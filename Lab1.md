# Lab1 Aspera High-Speed Transfer Server の準備

Lab1ではAspera High-Speed Transfer Server（以下、HST Server）のインストールを行うための準備を行います。  
設定する内容は下記になります。

- FWの設定変更  
- SELinuxの設定変更  
- インストール済みのパッケージをアップデート  

## FWの設定変更  
[ファイアーウォールの構成](https://download.asperasoft.com/download/docs/entsrv/3.9.3/es_admin_linux/webhelp/index.html#dita/configuring_the_firewall.html)

Asperaが使用するポートを許可します。  
- Outbound
  - 80:TCP
  - 443:TCP
  - 8080:TCP 
  - 8443:TCP  
- Inbound
  - 22:TCP *1
  - 33001:TCP
  - 33001:UDP
  - 8080:TCP
  - 8443:TCP
  - 80:TCP
  - 443:TCP
  - 9092:TCP

HTTPを使う場合、HTTPSを使う場合で一部異なりますが、今回は上記すべてを許可します。  

*1 Asperaでは接続開始時にssh接続を行います（FASPの仕様）よって、**port=22/tcp** を利用するのはセキュリティ上推奨されません（Asperaも推奨しません）
SSH接続ポートは**33001**に変更しますが、今回は設定の都合上 **port=22/tcp** のまま進めます。  


### 手順  
AsperaServer上で端末を開きます。  
端末上で下記のコマンドを実行します。  

```
su - 
# パスワードが求められるので、rootのパスワードを入力  

firewall-cmd --zone=public --add-port=33001/tcp --permanent
firewall-cmd --zone=public --add-port=33001/udp --permanent
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --zone=public --add-port=8443/tcp --permanent
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --add-port=443/tcp --permanent
firewall-cmd --zone=public --add-port=9092/tcp --permanent
firewall-cmd --reload
```

## SELinuxの設定変更  
[SELinuxを無効にする](https://download.asperasoft.com/download/docs/entsrv/3.9.3/es_admin_linux/webhelp/index.html#general_external/dita/linux/disabling_selinux.html)

Aspera HST ServerではSELinuxを無効にする必要があります。

### 設定方法
下記コマンドでSELinuxを無効にします。  
root ユーザーで実施します。

```
setenforce 0
sed -i".org" -e "s/^SELINUX=enforcing$/SELINUX=disabled/g" /etc/selinux/config
```
  
> `sed -i".org" -e "s/^SELINUX=enforcing$/SELINUX=disabled/g" /etc/selinux/config`  
> このコマンドは */etc/selinux/config* の *SELINUX=enforcing* を *SELINUX=disabled* に書き換えています。


## インストール済みのパッケージをアップデート
yumコマンドでパッケージをアップデートします。
> インターネットに接続できる必要があります。  

### 手順    
rootユーザーで実施します。  

```
yum update -y
```

## SSH Listeningポートの変更
Asperaでは制御用の通信に **SSH** を使用し、データ転送用通信に **33001/UDP** を使用します。  
セキュリティの観点から ssh をデフォルトの 22/tcp ではなく、33001/tcpが推奨されています。
ここでは、ssh listening portとして**33001**を許可します。
port **22**についてはハンズオンの最後で禁止の設定を行います。  

実作業としては */etc/ssh/sshd_config* の **#Port 22** を **Port 22**に変更しつつ、**Port 33001**も追記します。
全ての作業終了後、**Port 22** を再度 **#Port 22** に戻します。

### 手順  
下記のコマンドを実行します。  

```
sed -i".org" -e "/^#Port 22$/a Port 22" /etc/ssh/sshd_config
sed -i "/^#Port 22$/a Port 33001" /etc/ssh/sshd_config
systemctl restart sshd
``` 



以上が**Labo1**です。
[Lab2](https://github.com/keisz/aspera_handson/blob/master/Lab2.md) に進んでください。  

