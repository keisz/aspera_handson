## Lab2 Aspera HST Server のインストール  
Aspera HST Serverに必要なパッケージと本体をインストールします。  

手順概要は下記です。  

- ライセンスファイルのダウンロード
- 必要なパッケージのインストール  
- HST Serverのインストール  
- ライセンスの適用  
- httpd.confの変更  
- sudoersへの追記    
- Aspera用ユーザーの作成  

## ライセンスファイルのダウンロード  
HST Serverに適用するライセンスファイルを用意します。
別資料にてファイルのダウンロード先を案内します。  

個別でハンズオンを行う場合は別途入手が必要です。  



## 必要なパッケージのインストール  
Asperaを稼働するにはいくつかのLinuxのパッケージをインストールする必要があります。  
yumを使ってインストールします。  

### 手順  
下記コマンドを実行してインストールします。rootユーザーで実行します。  

```
su - 
# パスワードを求められますので root のパスワードを入力します。  

yum install -y httpd perl-Digest-MD5 perl
```


## Aspera HST Serverのインストール  
rpm コマンドもしくはyum コマンドを使ってAspera HST Serverをインストールします。  

### 参考URL
https://download.asperasoft.com/download/docs/entsrv/3.9.3/es_admin_linux/webhelp/index.html#dita/installing_the_product.html

### 手順  
下記コマンドを実行してインストールします。rootユーザーで実行します。  

```
su -
# パスワードを求められますので root のパスワードを入力します。  

rpm -Uvh /root/ibm-aspera-hsts-3.9.3.174419-linux-64.rpm
```

## ライセンス適用  
事前にダウンロードしたファイルをAspera HST Serverに適用します。  
適用方法は２つあります。

- GUIでファイルを読み込む  
- 特定のディレクトリにライセンスファイルを配置する  

今回はGUIでファイルを読み込む方法で実施します。  
作業はAspera ServerのVMにGUIでログインし適用します。

### 手順
1. Hyper-Vコンソールを立ち上げます。  

2. Aspera Serverのエントリを右クリックし、[接続]を選択します。ポップアップウインドウが開く、Aspera ServerのGUIコンソールが開きます。  

3. Userでログインします。ログインパスワードはHyper-Vホストのデスクトップ上においてある **config.txt** を確認してください。  

4. Aspera Serverのデスクトップ上で右クリックし**端末を開く**を選択します。   

5. 下記のコマンドを実行するとAsperaのGUIコンソールが表示されます。  
   `asperascp &`

6. 表示されたポップアップウインドウでダウンロードしたライセンスファイルを指定して適用します。  
   ダウンロードしたファイルは `/home/user/ダウンロード` にあります。フォルダーアイコンをクリックしディレクトリを変更して選択します。

7. ライセンス情報が表示されることを確認後、ウインドウを閉じます。  

8. Aspera のGUIコンソールが改めて表示されることを確認します。

#### CLIでライセンスを確認してみる 
端末上で `ascp -A` でライセンス情報を確認できます。  

```
# ascp -A
IBM Aspera High-Speed Transfer Server version 3.9.3.174419
ascp version 3.9.3.174419
Operating System: Linux
FIPS 140-2-validated crypto ready to configure
AES-NI Supported
Connect Server License max rate=10000 Mbps, account no.=13566, license no.=67795. Expiration date: Thu Dec 31 15:59:59 2020
Enabled settings: connect, mobile, cargo, node, drive, http_fallback_server, group_configuration, shared_endpoints, desktop_gui and sync2
```

## httpd.confの編集と起動  
AsperaではApacheを利用して、ユーザーが接続するためのWebUIを提供することができます。
事前にインストールしたコンポーネントに **httpd** がありますので、Apacheは稼働できる状態になっています。  
ここでは下記の設定を変更します。  

- ServerName にユーザーが接続するために利用するFQDNを指定。
- **UseCanonicalName OFF** を追記します。

### 手順  
1. Aspera Server上で端末を開き、`su -` でrootに変更します。  
2. **http.conf** に ServerNameを追加します。 今回は **asperaserver.local** と指定します。
    - 手動で変更する方法  
      ```
      vi /etc/httpd/conf/httpd.conf
      
      ## 表示された内容か該当箇所を変更  
      ## 95行目  
      #ServerName www.example.com:80
      ↓
      ServerName asperaserver.local:80
      ```

    - ワンラインで変更  
      ```
      cp -p /etc/httpd/conf/httpd.conf /etc/httpd/conf/httpd.conf.`date +"%Y%m%d_%H%M%S"` && sed -i "s/^#ServerName www.example.com:80/ServerName asperaserver.local:80/g" /etc/httpd/conf/httpd.conf
      ```

3. `UseCanonicalName off` を追記します。
    - 手動で変更する方法  
      ```
      vi /etc/httpd/conf/httpd.conf
      
      ## 最終行に追記します。  
      UseCanonicalName off
      ```

    - ワンラインで変更  
      ```
      sed -i '$a UseCanonicalName off' /etc/httpd/conf/httpd.conf
      ```

4. Asperaの設定を追記します。
    - 手動で追記 
      ``` 
      vi /etc/httpd/conf/httpd.conf

      ## 最終行に追記します。  
      #BEGIN_ASPERA
      <Directory /opt/aspera/var/webtools>
      AllowOverride All
      Allow from all
      </Directory>
      <Directory /opt/aspera/var/webtools/scripts>
         AddHandler cgi-script .pl
         SetHandler cgi-script
         Options +ExecCGI
         AllowOverride All
      </Directory>
      ScriptAlias /aspera/scripts/ "/opt/aspera/var/webtools/scripts/"
      Alias /aspera/ "/opt/aspera/var/webtools/"
      #END_ASPERA
      ```
    - コマンドで1発変更  
      ```
      cat <<EOL >> /etc/httpd/conf/httpd.conf
      <Directory /opt/aspera/var/webtools>
      AllowOverride All
      Allow from all
      </Directory>
      <Directory /opt/aspera/var/webtools/scripts>
         AddHandler cgi-script .pl
         SetHandler cgi-script
         Options +ExecCGI
         AllowOverride All
      </Directory>
      ScriptAlias /aspera/scripts/ "/opt/aspera/var/webtools/scripts/"
      Alias /aspera/ "/opt/aspera/var/webtools/"
      #END_ASPERA
      EOL
      ```
5. httpd serviceを起動します。  
   `systemctl start httpd`
   すでに起動している場合は `systemctl restart httpd` で再起動します。  
   サービスのステータス確認は `systemctl status httpd` 

## System-Level セキュリティ設定  
GUIベースで接続する際のセキュリティ設定になります。

### 手順  
1. 下記のコマンドを実行します。  
   `sudo /opt/aspera/sbin/enablesecure enable`  
   ```
   # sudo /opt/aspera/sbin/enablesecure enable
   Enter user running apache (default: apache):

   Successfully enabled IBM Aspera High-Speed Transfer Server system-level security!

   In order to complete this operation, add the following to your /etc/sudoers file.

   # BEGIN IBM Aspera High-Speed Transfer Server
   # The user account that runs the web server will impersonate
   # the logged-in user to present that user's files and folders.
   Defaults env_keep += "SERVER_NAME REQUEST_URI REQUEST_METHOD REMOTE_USER QUERY_STRING CONTENT_LENGTH SESSION_ID CSRF_TOKEN"
   Defaults:apache !requiretty
   apache ALL=(ALL) NOPASSWD: /opt/aspera/var/webtools/scripts/aspera-dirlist.pl, SETENV: /opt/aspera/var/webtools/scripts/aspera-dirlist.pl
   # END IBM Aspera High-Speed Transfer Server
   ```
2. 先ほどのコマンドで出力された内容を **sudoers** ファイルの最後に追記します。  
    - 手動で変更する方法  
      ```
      vi /etc/sudoers
      
      ## ファイルの最後に下記を追記  
      ## 保存は wq! でします  

      # BEGIN IBM Aspera High-Speed Transfer Server
      # The user account that runs the web server will impersonate
      # the logged-in user to present that user's files and folders.
      Defaults env_keep += "SERVER_NAME REQUEST_URI REQUEST_METHOD REMOTE_USER QUERY_STRING CONTENT_LENGTH SESSION_ID CSRF_TOKEN"
      Defaults:apache !requiretty
      apache ALL=(ALL) NOPASSWD: /opt/aspera/var/webtools/scripts/aspera-dirlist.pl, SETENV: /opt/aspera/var/webtools/scripts/aspera-dirlist.pl
      # END IBM Aspera High-Speed Transfer Server

      ```

    - コマンドで1発変更
      ```
      cat <<EOL >> /etc/sudoers
      # BEGIN IBM Aspera High-Speed Transfer Server
      # The user account that runs the web server will impersonate
      # the logged-in user to present that user's files and folders.
      Defaults env_keep += "SERVER_NAME REQUEST_URI REQUEST_METHOD REMOTE_USER QUERY_STRING CONTENT_LENGTH SESSION_ID CSRF_TOKEN"
      Defaults:apache !requiretty
      apache ALL=(ALL) NOPASSWD: /opt/aspera/var/webtools/scripts/aspera-dirlist.pl, SETENV: /opt/aspera/var/webtools/scripts/aspera-dirlist.pl
      # END IBM Aspera High-Speed Transfer Server
      EOL
      ```    


## Aspera用ユーザー作成  
Asperaでファイル転送する際に利用するユーザーを作成します。ユーザーはOSユーザーをベースとして設定します。  
今回は下記のようなユーザーを作成します。  

group: aspgroup
user1  
username: aspuser01
password: aspera
user2
username: aspuser02
password: aspera

### 手順  
下記コマンドを実行します。  

```
groupadd -r aspgroup
useradd -r aspuser01 -s /bin/aspshell -g aspgroup
useradd -r aspuser02 -s /bin/aspshell -g aspgroup

passwd aspuser01
# パスワード aspera を2回入力
passwd aspuser02
# パスワード aspera を2回入力
```

## Home Dirの作成  
作成したユーザー用のHome Dirを作成します。  

### 手順  
下記コマンドを実行します。

```
mkdir -p /home/aspuser01
mkdir -p /home/aspuser02
chown aspuser01:aspgroup /home/aspuser01
chown aspuser02:aspgroup /home/aspuser02
```

## ファイル転送のDocumentRootの作成  
ファイル転送でディレクトリを選択する際の起点となるDocumentRootを作成します。 

### 手順  

```
mkdir -p /docroot
chown aspuser01:aspgroup /docroot
chown aspuser02:aspgroup /docroot
```

## Asperaへのユーザー登録  
作成したOSユーザーをAsperaユーザーとして利用できるように登録します。  
ユーザーの登録はGUIにて行います。  

### 手順  
1. AsperaServerのGUIにログインし、端末を開きます。  
2. 下記コマンドを実行してAsperaのGUIコンソールを開きます。  
   ```
   su -
   #パスワードを入力  
   asperascp &
   ```
3. [構成]をクリックします。

4. [ユーザー]タブを開き、[＋]をクリックします。

5. **aspuser01** と入力し、[ok]をクリックします。

6. [docroot]タブで **絶対パス** のオーバーライドにチェックをつけ、有効な値に **/docroot** と入力します。  

7. [帯域幅]のタブに移り、下記の設定をします。記載のない設定は変更しません。  
   - 受信目標速度の限界値 (Kbps)
     - オーバーライド: チェックを付ける  
     - 有効な値: 1000000
   - 送信目標速度の限界値 (Kbps)
     - オーバーライド: チェックを付ける  
     - 有効な値: 1000000

8. 構成画面の[適用]をクリックし[OK]で完了します。  

## aspera.confへの追記  
**aspera.conf**に設定を追記します。  

### 手順  
1. vimでaspera.conf ファイルを開きます。  
   `vim /opt/aspera/etc/aspera.conf`

2. *<central_server>の前にエントリを追加します。  
   追加する内容
   ```
    <WEB
    SshPort = "33001"
    UdpPort = "33001"
    PathMTU = "0"
    HttpFallback="no"
    HttpFallbackPort="8080"
    HttpsFallbackPort="8443"
    EnableDelete = "yes"
    EnableCreateFolder = "yes"
    EnableUserSwitching = "yes"
     />
    <http_server>
      <http_port>8080</http_port>
      <https_port>8443</https_port>
      <enable_http>true</enable_http>
      <enable_https>true</enable_https>
     </http_server>
   ```

3. Asperahttpdを再起動します。
   `systemctl restart asperahttpd`

## ユーザーアカウントの有効化  
作成したユーザーがAsperaのWebUIに接続する際の認証用パスワードを設定します。  

### 手順  
下記のコマンドを実行します。  

```
su -
# パスワードを入力します。  
htpasswd /opt/aspera/etc/webpasswd aspuser01
# パスワード aspera を2回入力します。
htpasswd /opt/aspera/etc/webpasswd aspuser02
# パスワード aspera を2回入力します。
```

以上でAspera HST Serverのインストールが終わりました。
次はLab3に進んでいただき、クライアントの設定を行います。

