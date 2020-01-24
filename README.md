# Aspera ハンズオンテキスト
この文書は IBM Asperaのハンズオン用の資料です。
ハンズオンには製品バイナリおよびライセンスが必要になります。

ハンズオンを実施したい場合は別途下記までご連絡ください。
- ibm-info@networld.co.jp  

## 環境  
全てのシナリオを実行するには下記の環境が必要になります。
ネットワールド社で実施するハンズオンでは、Azure Lab Serviceを利用し、1台のHyper-Vサーバー上に3台のVMを動かしています。  

- Aspera High-Speed Transfer Server  : 1台  
- Asperaクライアント  : 2台 

スペックは下記をご確認ください。

- Aspera High-Speed Transfer Server
  - hostname: asperaserver.local
  - OS:CentOS 7.7
  - CPU:
  - memory: 8GB
  - Disk: 40GB
- Aspera クライアント(1台目) 
  - hostname: centos.local
  - OS: CentOS 7.7
  - CPU:
  - memory: 8GB
  - Disk: 40GB
- Aspera クライアント(2台目) 
  - hostname: windows10
  - OS: Windows 10 
  - CPU:
  - memory: 8GB
  - Disk: 60GB

機器（VM）はすべてインターネット接続が可能としています。  

## アジェンダ
Aspera High-Speed Transfer Serverのインストールと利用方法をWindows/Linuxどちらからも試すことができます。

- [Lab1 Aspera High-Speed Transfer Server の準備](https://github.com/keisz/aspera_handson/blob/master/Lab1.md)
- [Lab2 Aspera HST Server のインストール](https://github.com/keisz/aspera_handson/blob/master/Lab2.md)
- [Lab3 Windows クライアントにAspera Connectの導入とアップロード/ダウンロード試験](https://github.com/keisz/aspera_handson/blob/master/Lab3.md)
- [Lab4 Windows クライアントにAspera Desktopの導入とアップロード/ダウンロード試験](https://github.com/keisz/aspera_handson/blob/master/Lab4.md)
- [Lab5 Linuxクライアントを使ったAspera CLIによるファイル転送](https://github.com/keisz/aspera_handson/blob/master/Lab5.md)
- [Lab6 Aspera Console のインストールと設定](https://github.com/keisz/aspera_handson/blob/master/Lab6.md)


