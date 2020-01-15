# Lab6 Aspera Console のインストールと設定   
IBM Aspera Console Application は、Web ベースの管理アプリケーションです。Aspera 環境に対する完全な可視性を提供するとともに、転送、ノード、およびユーザーに対するほぼリアルタイムの中央制御を可能にし、カスタマイズされたレポートおよび監査の広範囲にわたるロギングを維持します。

このLabでは、Aspera Consoleをインストールし、これまでのLabで作成した Aspera HST Serverを管理します。

## Aspera Consoleのインストール   
Aspera Console のインストールとインストールに必要なパッケージの導入などを行います。  
Aspera ConsoleのプログラムはLabのVM **CentOS** に導入します。  

### 手順  
1. Hyper-Vコンソールを立ち上げます。  

2. CentOSのエントリが実行中となっていることを確認します。 
> CentOSの状態が実行中となっていない場合はエントリを右クリックし**起動**を実行してください。

3. デスクトップ上にある TeraTermのショートカット **CentOS** を実行します。  

4. Consoleが立ち上がります。  
   Aspera Consoleのrpmファイルが保存してあることを確認します。  
   ファイルは **/work** に保存しています。  
   `ls -la /work`

   ```
   # ls
   ibm-aspera-common-1.2.25.170459-0.x86_64.rpm
   ibm-aspera-console-3.3.3.171427-0.x86_64.rpm
   ```

5. インストールに必要な **Perl** をインストールします。  
   `yum install -y perl`

   ```
   # yum install -y perl
   “Ç‚Ýž‚ñ‚¾ƒvƒ‰ƒOƒCƒ“:fastestmirror, langpacks
   Determining fastest mirrors
    * base: ftp.iij.ad.jp
    * extras: ftp.iij.ad.jp
    * updates: ftp.iij.ad.jp
   base                                                     | 3.6 kB     00:00
   extras                                                   | 2.9 kB     00:00
   updates                                                  | 2.9 kB     00:00
   (1/2): extras/7/x86_64/primary_db                          | 153 kB   00:00
   (2/2): updates/7/x86_64/primary_db                         | 5.9 MB   00:00
   ƒpƒbƒP[ƒW 4:perl-5.16.3-294.el7_6.x86_64 ‚ÍƒCƒ“ƒXƒg[ƒ‹Ï‚Ý‚©ÅVƒo[ƒWƒ‡ƒ“‚Å‚·
   ‰½‚à‚µ‚Ü‚¹‚ñ   
   ```

6. Aspera Consoleをインストールします。  
   
   `rpm -Uvh /work/ibm-aspera-common-1.2.27.176566-0.x86_64.rpm`


   ```
   # rpm -Uvh /work/ibm-aspera-common-1.2.27.176566-0.x86_64.rpm
   準備しています...              ################################# [100%]
   更新中 / インストール中...
      1:aspera-common-1.2.27.176566-0    ################################# [100%]
   Created symlink from /etc/systemd/system/multi-user.target.wants/aspera_db_space_watcher.service to /usr/lib/systemd/system/aspera_db_space_watcher.service.
   ```

   `rpm -Uvh /work/ibm-aspera-console-3.4.0.176566-0.x86_64.rpm`

   ```
   # rpm -Uvh /work/ibm-aspera-console-3.4.0.176566-0.x86_64.rpm
   準備しています...              ################################# [100%]
   更新中 / インストール中...
      1:aspera-console-3.4.0.176566-0    ################################# [100%]
   To complete the setup of Console, run "asctl console:setup"
   ```


7. Setupコマンドの実行  
   rpmのインストール後に下記メッセージが表示されていますので、コマンドを実行し、Setupを進めます。

   > To complete the setup of Console, run "asctl console:setup"

   `asctl console:setup`

   ```
   # asctl console:setup

   Streamlined: A simple setup with all components running on this computer.
   Detailed:    An advanced configuration (e.g. non-standard ports or not all components running on a single server)

   Streamlined or detailed setup (s/d)? (current: s) s  #←「s」を入力

   Console
     Choose a login name for the new admin user (recommendation: don't use 'admin' or 'root'): aspadmin  #←ユーザー名を入力
     Enter the email address for aspadmin: aspadmin@testdomain.com  #←メールアドレス（今回はダミー）を入力
     Enter the password for aspadmin:
         Password: **********  #←パスワードを入力（デスクトップ上のConfig確認)
          Confirm: **********  #←パスワードを入力（デスクトップ上のConfig確認)

   MySQL
     Please enter a new MySQL root password.
         Password: **********  #←パスワードを入力（デスクトップ上のConfig確認)
          Confirm: **********  #←パスワードを入力（デスクトップ上のConfig確認)
     MySQL will need to start/restart during configuration. Continue (y/n)? (current: y) Y  #←「y」を入力

   Apache
     What hostname or IP address should Apache use to identify itself (in the SSL Certificate)? centos.local  #← 同じように入力
       Key and certificate will be generated in this directory:
         /opt/aspera/common/apache/conf
     What IP address will managed nodes use to log to the database (by default)? 192.168.0.20  #← 同じように入力

   ===================== Settings =====================
   MySQL
     Enabled:     true
     Port:        4406
   Apache
     Enabled:          true
     Hostname:         centos.local
     Bind Address:     0.0.0.0
     HTTP Port:        80
     HTTPS Port:       443
   Console
     Enabled:            true
     DB Logger IP:       192.168.0.20
     Admin name:         aspadmin
     Admin email:        aspadmin@testdomain.com
     MySQL is local:     true

   Are these settings correct? (y/n/x with x for exit) y
   MySQL
     Recording system user and group...                           done
     Enabling MySQL...                                            done
     Installing startup script...                                 done
     Setting up db space watcher...                               done
     Creating 'mysql'...                                          done
     Granting temporary shell access...                           done
     Setting database dir...                                      done
     Creating required directories...                             done
     Setting permissions...                                       done
     Creating database...                                         done
     Securing daemon user account...                              done
     Backing up config files...                                   Backing up config files to /opt/aspera/common/mysql/my_template.ini.bak, /opt/aspera/common/mysql/share/mysql/mysql.server.bak
   done
     Setting port...                                              done
     Generating config files...                                   done
     Setting permissions...                                       done
     Stopping MySQL if running...                                 done
     Starting MySQL...                                            done
     Setting MySQL password...                                    done
     Granting privileges...                                       done
     Updating version...                                          done
   Apache
     Recording system user and group...                           done
     Pre-execution checks...                                      done
     Enabling Apache...                                           done
     Installing startup script...                                 done
     Creating system user...                                      done
     Creating required directories...                             done
     Setting IP...                                                done
     Setting hostname...                                          done
     Setting HTTP port...                                         done
     Setting HTTPS port...                                        done
     Generating SSL key...                                        done
     Generating configuration file...                             done
     Checking ashttpd user...                                     done
     Updating version...                                          done
   Console
     Enabling console...                                          done
     Recording system user and group...                           done
     Creating required directories...                             done
     Creating system user...                                      done
     Securing files...                                            done
     Installing startup script...                                 done
     Setting Default Database Logger IP...                        done
     Generating configuration files...                            done
     Using local MySQL...                                         done
     Starting database server...                                  done
     Creating/Verifying database configuration...                 done
     Migrating console database (this may take a while)...        done
     Creating an admin user...                                    done
     Generating email templates...                                done
     Registering with Apache...                                   done
     Setting ownership and permissions...                         done
     Encrypting database passwords...                             done
     Task Name                                                 Status
   ==================================================================
   MySQL
     Recording system user and group                              OK
     Enabling MySQL                                               OK
     Installing startup script                                    OK
     Setting up db space watcher                                  OK
     Creating 'mysql'                                             OK
     Granting temporary shell access                              OK
     Setting database dir                                         OK
     Creating required directories                                OK
     Setting permissions                                          OK
     Creating database                                            OK
     Securing daemon user account                                 OK
     Backing up config files                                      OK
     Setting port                                                 OK
     Generating config files                                      OK
     Setting permissions                                          OK
     Stopping MySQL if running                                    OK
     Starting MySQL                                               OK
     Setting MySQL password                                       OK
     Granting privileges                                          OK
     Updating version                                             OK
   Apache
     Recording system user and group                              OK
     Pre-execution checks                                         OK
     Enabling Apache                                              OK
     Installing startup script                                    OK
     Creating system user                                         OK
     Creating required directories                                OK
     Setting IP                                                   OK
     Setting hostname                                             OK
     Setting HTTP port                                            OK
     Setting HTTPS port                                           OK
     Generating SSL key                                           OK
     Generating configuration file                                OK
     Checking ashttpd user                                        OK
     Updating version                                             OK
   Console
     Enabling console                                             OK
     Recording system user and group                              OK
     Creating required directories                                OK
     Creating system user                                         OK
     Securing files                                               OK
     Installing startup script                                    OK
     Setting Default Database Logger IP                           OK
     Generating configuration files                               OK
     Using local MySQL                                            OK
     Starting database server                                     OK
     Creating/Verifying database configuration                    OK
     Migrating console database (this may take a while)           OK
     Creating an admin user                                       OK
     Generating email templates                                   OK
     Registering with Apache                                      OK
     Setting ownership and permissions                            OK
     Encrypting database passwords                                OK

   Apache needs to be restarted, restart it now (y/n)? (default:y) y  #←「y」を入力
   Apache: Start... done
   MySQL needs to be restarted, restart it now (y/n)? (default:y) y  #←「y」を入力
   MySQL: Stop... done
   MySQL: Start... done
   Console needs to be restarted, restart it now (y/n)? (default:y) y  #←「y」を入力
   Console: Start... done
   Reminders:
    - Apache needs ports 80 and 443 open in firewalls.
    - No Console license found.  Please refer to the documentation to install your Console license.
   ```


8. FWの設定変更  
Apacheで **80** と **443** のポートを利用するので、下記コマンドを実行してFWの設定を変更します。  

```
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --add-port=443/tcp --permanent
firewall-cmd --reload
```

9. Webブラウザでのアクセス確認
Hyper-Vホストマシン上でFirefoxを開き、下記URLにアクセスします。  

> https://192.168.0.20/

HTTPSのエラーを **危険を承知で続行** で進み、Aspera Consoleのログイン画面が表示されることを確認します。  


## Aspera Console ライセンス登録  
Aspera Cosoleではインストール後の初回ログイン時にライセンス登録とSetupで作成したユーザーのパスワードを変更する必要があります。  

### 手順  

1. Hyper-Vホスト上でFirefoxを起動し、 **https://192.168.0.20/** にアクセスします。HTTPSのエラーなどは回避してログイン画面まで進みます。  
   
2. ログイン画面で下記の情報を入力します。  
   - user: aspadmin
   - password: aspadmin

3. ログイン後、ライセンスの入力画面が表示されます。  **Upload a license file** をクリックし、下記のファイルを指定します。  
   - C:\Users\aspera_admin\Desktop\Aspera_Hands_on\License\aspera.Console.67796.license  

4. ライセンスが正常にUploadされるとユーザーのパスワード変更画面に遷移します。  
   現在のパスワードと新しいパスワードを2回入力します。  
   
   - 現在のパスワード: aspadmin   
   - 新しいパスワード: Aspera123!

5. Aspera ConsoleのLicense画面に遷移します。ほかのメニュー等も表示できることを確認します。  


## Aspera HSTサーバーの登録  
Lab2で作成したHSTサーバーをAspera Consoleに登録します。  
登録にあたり、HSTサーバー（VM: asperaserver）で作業が必要になります。作業実施対象を間違えないように注意してください。  

### 手順
#### HSTサーバー（VM: asperaserver）で作業

1. Aspera Consoleと連携用のユーザーアカウントを作成します。  
   - username: console_admin
   - password: Aspera123

   `useradd console_admin -s /bin/aspshell`
   `passwd console_admin`

   ```
   # useradd console_admin -s /bin/aspshell
   # passwd console_admin
   ƒ†[ƒU[ console_admin ‚ÌƒpƒXƒ[ƒh‚ð•ÏXB
   V‚µ‚¢ƒpƒXƒ[ƒh:
   V‚µ‚¢ƒpƒXƒ[ƒh‚ðÄ“ü—Í‚µ‚Ä‚­‚¾‚³‚¢:
   passwd: ‚·‚×‚Ä‚Ì”FØƒg[ƒNƒ“‚ª³‚µ‚­XV‚Å‚«‚Ü‚µ‚½B
   ```

   > このユーザーもshellを **aspshell** に変更しているのでOSにログインできません。

2. aspera.confの所有者を変更します。  
   
   `chown console_admin:console_admin /opt/aspera/etc/aspera.conf`

3. Node API ユーザーを作成します。  
   Aspera ConsoleではNode API ユーザーを利用して、Node（Aspera HST Server)を監視します。  
   そのため、Aspera ConsoleにHSTサーバーを追加する際には、Node APIユーザーが必要になります。  

   Node API ユーザー作成時に 1. で作成したユーザー **console_admin** も利用します。  

   - Node API ユーザー: node_admin
   - password: Aspera123
   
   `/opt/aspera/bin/asnodeadmin -a -u node_admin -p Aspera123 -x console_admin --acl-set impersonation`

4. Node API ユーザーを確認します。  
   `/opt/aspera/bin/asnodeadmin -l`

   ```
   # /opt/aspera/bin/asnodeadmin -l
   List of node user(s):
                   user       system/transfer user                    acls
   ====================    =======================    ====================
             node_admin              console_admin    [impersonation]
   ```

#### Aspera Console（VM: centos）で作業
1. Hyper-Vホスト上でFirefoxを起動し、 **https://192.168.0.20/** にアクセスします。HTTPSのエラーなどは回避してログイン画面まで進みます。  
   
2. ログイン画面で下記の情報を入力します。  
   - user: aspadmin
   - password: Aspera123!

3. ログイン後、**Nodes** に移動し、**New Managed Node** をクリックします。  


4. Creating New Nodeの画面に移動します。下記の情報を入力し、 **Create** をクリックします。記載のない設定はデフォルトのまま進めます。  
   |設定名|値| 
   |:--------|:----------------|
   |IP Address|192.168.0.10|
   |Name|Asperaserver.local|
   |SSH Port|33001|
   

5. Credentials の設定に移動します。先ほどasperaserverで作成したユーザーの情報を入力し、 **Update** をクリックします。    

   |設定名|値
   |:---|:---|
   |SSH
   |Username|console_admin|
   |Password|Aspera123|
   |Node API
   |Username|node_admin|
   |Password|Aspera123|

6. Credentials updated. のメッセージが表示されることを確認します。　　

