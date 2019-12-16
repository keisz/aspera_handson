## Lab2 Aspera HST Server のインストール  
Aspera HST Serverに必要なパッケージと本体をインストールします。  

手順概要は下記です。  

- 必要なパッケージのインストール  
- ファイルのコピー
- HST Serverのインストール  
- ライセンスの適用  
- httpd.confの変更  
- sudoersへの追記    
- Aspera用ユーザーの作成  

## 必要なパッケージのインストール  
Asperaを稼働するにはいくつかのLinuxのパッケージをインストールする必要があります。  
yumを使ってインストールします。  

### 設定方法  
下記コマンドを実行してインストールします。rootユーザーで実行します。  

```
su - 
# パスワードを求められますので root のパスワードを入力します。  

yum install -y httpd perl-Digest-MD5 perl
```

## ファイルのコピー  
Hyper-Vホスト上に用意されているファイルを**AsperaServer**上にコピーしインストールします。  
ファイルは以下のものがあります。  

- ibm-aspera-hsts-3.9.3.174419-linux-64.rpm
- 57254-EntSrv-Faspex-20M.aspera-license





