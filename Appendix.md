## Aspera GUIコンソールの日本語化  
Aspera GUIコンソールが日本語で表示されない場合、環境変数をセットしてコンソールを開くことでコンソールが日本語化されます。  

### 手順
端末上で下記を実行します。
```
export LANG=ja_JP.UTF-8
asperascp &
```

## ワンラインでSSH Listen Portを 22 → 33001 に変更する
Asperaの推奨設定に変更します。これ以降、sshで接続するにはport指定が必要になります。

### 手順
```
SSH_PORT=33001 && cp -p /etc/ssh/sshd_config /etc/ssh/sshd_config.`date +"%Y%m%d_%H%M%S"` && sed -i "s/^#Port 22/Port ${SSH_PORT}/g" /etc/ssh/sshd_config && systemctl restart sshd
```


## ワンラインでSSH Listen Portに 33001 を追加する
- 追加すると自動的に port 22 にsshで接続できなくなります。  

### 手順
```
SSH_PORT=33001 && cp -p /etc/ssh/sshd_config /etc/ssh/sshd_config.`date +"%Y%m%d_%H%M%S"` && sed -i "/#AddressFamily/i Port ${SSH_PORT}" /etc/ssh/sshd_config && systemctl restart sshd
```

## ワンラインでSSH Listen Portの 22 を有効化する  
- デフォルトで `#Port 22` となっているエントリを使います。  

### 手順
```
cp -p /etc/ssh/sshd_config /etc/ssh/sshd_config.`date +"%Y%m%d_%H%M%S"` && sed -i "s/^#Port 22/Port 22/g" /etc/ssh/sshd_config && systemctl restart sshd
```

## Aspera Console の再起動  
- Aspera Console の各サービスを再起動コマンドが用意されています。

### 手順
`asctl console:restart`

## AsperaのFAQ  
AsperaのドキュメントはIBM Knowledge Centerとは別の場所で公開されています。
OSやバージョンによりリンクが異なる点とHTMLとPDFのガイドが公開されています。  
英語のみで日本語での提供はされていません。

- Aspera Document のルート
  - [https://downloads.asperasoft.com/documentation/](https://downloads.asperasoft.com/documentation/)
- Aspera High-Speed Transfer Server
  - [https://downloads.asperasoft.com/en/documentation/1](https://downloads.asperasoft.com/en/documentation/1)
- Aspera Desktop Client
  - [https://downloads.asperasoft.com/en/documentation/2](https://downloads.asperasoft.com/en/documentation/2)
- Aspera CLI
  - [https://downloads.asperasoft.com/en/documentation/62](https://downloads.asperasoft.com/en/documentation/62)
- Aspera Console
  - [https://downloads.asperasoft.com/en/documentation/3](https://downloads.asperasoft.com/en/documentation/3)


## Aspera DesktopのGUIの違い
Windows版とLinux版でほぼ違いはありません。

## Aspera 製品のダウンロードの可否 
Asperaでは、Asperaのサーバー機能（High Speed Transfer Serverなど）に接続するためのクライアント製品は課金対象ではありません。  
Aspera DesktopやCLI、Aspera Connectは誰でもダウンロードができます。

- https://downloads.asperasoft.com/documentation/

High Speed Transfer ServerやAspera Consoleについては課金対象となるアプリケーションのため、製品ライセンスを購入したユーザーのみがダウンロードすることができます。
  

## Asperaでファイル転送時に速度がでない or 45Mbps近辺でとまる場合
転送速度のデフォルト値を指定しないもしくは低速で指定していることが想定されます。
Asperaのクライアント側で指定もしくはサーバーサイドで転送開始の初期値を設定してください。

ハンズオンでは、HST ServerとAspera Desktop Clientに対して設定する方法を紹介しています。

## Aspera HST Serverの管理コンソールでメニューがグレーアウトされてしまう
コンソールを実行するときの端末（Console）の実行ユーザーがユーザー権限になっています。
`su -`でroot権限に変更するか、 `sudo` をつけて実行してください。  

