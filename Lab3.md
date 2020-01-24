# Lab3 Windows クライアントにAspera Connectの導入とアップロード/ダウンロード試験   
WindowsクライアントにAspera Connectを導入しファイル転送のテストをします。
作業概要は下記になります。

- Aspera Connect の導入
- WebUIでファイルのアップロード
- WebUIでファイルのダウンロード
- 転送速度の検証
- 転送速度設定

## Aspera Connect の導入  
Hyper-V上に用意されているWindows 10を利用し、Aspera Connectをインストールします。  

### 手順  

1. Hyper-Vコンソールを立ち上げます。  

2. **Windows10** のエントリを右クリックし、[接続]を選択します。ポップアップウインドウが開く、Windows10のGUIコンソールが開きます。  
> Windows10の状態が実行中となっていない場合はWindows10を起動してください。  

3. Userでログインします。ログインパスワードはHyper-Vホストのデスクトップ上においてある **config.txt** を確認してください。  

1. デスクトップ上のリンクからFirefoxを起動し、下記URLにアクセスします。  
   - http://asperaserver.local/aspera/user

2. 認証画面が表示されますので、Lab2で作成したユーザーアカウントを入力します。
   - user: aspuser01
   - password: aspera
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/20-1.png)
   
3. AsperaのWebUIが開き、ポップアップウインドウが表示されます。  
   **アドオンのインストール**をクリックします。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/21-1.png)
   
4. Firefoxの別タブが開き、Extensionのインストールが求められます。 **Add to Firefox** をクリックします。　
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/22-1.png)
   
5. 確認画面が表示されますので **追加** をクリックします。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/23-1.png)
   
6. WebUIタブに戻ります。右上にAdd-on追加のメッセージが表示されますので、**ok**をクリックし、**アプリケーションのダウンロード**のリンクをクリックします。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/27-1.png)
   
7. ファイルのダウンロードのポップアップウインドウが表示されます。 **ファイルの保存**をクリックします。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/28-1.png)
   
8. **↓**のアイコンをクリックするとダウンロードしたファイルが表示されますので、ファイルをクリックします。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/29-1.png)
   
9. *IBM Aspera Connect のセットアップ*が表示されますので、**開始**をクリックします。 
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/30-1.png)
   
10. *インストールが完了しました* が表示されたら閉じるをクリックします。
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/33-1.png)


> WebUI接続中にAspera Connectのインストール確認の画面が表示されることが（多々）あります。  
> これは、ローカル上にAspera Connectがインストールされていることを確認しているためです。  
> インストールされていれば表示は消えますのでしばらくお待ちください。
> 表示中は一部操作に制限（アップロードやダウンロードアイコンが表示されません）があります。


## WebUI上からファイルをアップロードしてみる  
Aspera HST Serverに接続する準備ができたので、WebUIに接続し、ファイルをアップロードしてみます。  


### 手順  
1. Firefoxで下記URLにアクセスします。  
   - http://asperaserver.local/aspera/user

2. 認証画面が表示されますので、Lab2で作成したユーザーアカウントを入力します。
   - user: aspuser01
   - password: aspera

3. Aspera WebUIが表示されます。 *アップロード*のアイコンをクリックします。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/38-1.png)

4. ファイルの選択画面が表示されます。 デスクトップに配置してある **テストファイル1.txt** を選択し、**開く**をクリックします。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/34-1.png)

5. 確認のポップアップが表示されます。 **許可**をクリックします。　　
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/40-1.png)

6. ログイン画面が表示されます。ここではLab2で作成したLinux OSユーザーのIDとパスワードが必要になります。 **このパスワードを記憶する**にもチェックを付けておきます。
   - user: aspuser01
   - password: aspera
  ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/36-1.png)

7. アクティビティー画面が表示されます。転送が開始され、完了と表示されたらファイルの転送が完了です。 **x** ボタンでウインドウを閉じます。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/37-1.png)

8. Aspera WebUI上にファイルが表示されていない場合は **F5** で画面を更新します。  
   更新後、**テストファイル1.txt** がリストされていることを確認します。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/39-1.png)


## WebUIからファイルをダウンロードしてみる  
先ほどアップロードしたファイルをダウンロードしてみます。  

### 手順  
1. Firefoxで下記URLにアクセスします。  
   - http://asperaserver.local/aspera/user

2. 認証画面が表示されますので、Lab2で作成したユーザーアカウントを入力します。
   - user: aspuser01
   - password: aspera

3. Aspera WebUIが表示されます。リストされている **テストファイル1.txt** にチェックをいれ、**ダウンロード** ボタンをクリックします。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/39-1.png)

4. 確認のポップアップが表示されます。 **許可**をクリックします。
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/40-1.png)

5. アクティビティー画面が表示されます。転送が開始され、完了と表示されたらファイルの転送が完了です。 **x** ボタンでウインドウを閉じます。
   - 認証画面が出た場合はLab2で作成した Linux OS ユーザーのIDとパスワードを入力します。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/42-1.png)

6. ダウンロード先はOSログインユーザーの*ダウンロード*フォルダになります。
   - C:\Users\user\download
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/43-1.png)
   

## WebUIからフォルダをアップロードしてみる  
次に複数のファイルが保存されたフォルダをアップロードしてみます。  
フォルダはデスクトップ上用意されている **テストフォルダ** を利用します。  

### 手順  
1. Aspera WebUIにログインし、**フォルダーのアップロード** をクリックします。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/44-1.png)
   
2. デスクトップ上の **テストフォルダ** を選択します。
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/45-1.png)

3. 確認のポップアップが表示されます。 **許可**をクリックします。
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/40-1.png)
   
4. アクティビティー画面が表示されます。転送が開始され、完了と表示されたらファイルの転送が完了です。 **x** ボタンでウインドウを閉じます。
   - 認証画面が出た場合はLab2で作成した Linux OS ユーザーのIDとパスワードを入力します。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/47-1.png)

5. WebUI画面を更新し、テストフォルダがリストされていること、ドリルダウンしてファイルが閲覧できることを確認します。    
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab3/48-1.png)

## 転送速度の検証  
サイズの大きいファイルを転送し、アップロード時にどのくらいの速度がでるか検証します。  
ファイルは **1GB/5GB/10GB** を用意しています。それぞれをアップロードしてみます。  

### 手順  
1. Aspera WebUIにログインします。Rootディレクトリに移動し、**新規フォルダー**をクリックします。  

2. **速度検証**という名前でフォルダーを作成します。  

3. リストされている*速度検証*のフォルダをクリックし **アップロード** アイコンをクリックします。  

4. デスクトップ上の *testfile_1GB* を選択し開きます。確認画面も許可します。

5. アクティビティーウインドウにステータスが表示されます。 **[~]**のアイコンをクリックします。  

6. 転送モニターが開き、転送時の速度、経過が表示されます。  

7. 完了後、転送モニターを [x]で閉じます。  

8. 5GBファイル、10GBファイルもデスクトップ上に用意していますので同様にアップロードを行います。  

> **転送モニター**  
> 転送モニターでは、転送速度の上限値、下限値を変更することができます。
> モニター左側の**[▶]** をドラッグし上限に動かせます。  
> 下記画像では上限値を350Mbpsあたりまで下げています。Asperaの転送速度の最大値はもう少し下ですので、300Mbps前後が最大値で転送が行われていることが確認できます。  

9. 3ファイルがリストされていることを確認します。  

10. ログを確認します。Aspera Connectのアイコンを右クリックし、**ログ・フォルダーを開く**を選択します。  

11. リストされているログファイルの **aspera-scp-transger.*** のファイルを開きます。  

12. 最終行から遡り、最初の改行を見つけます。改行後のログから最終行までが1つのファイル転送ログになっています。  
    おそらく**testfile_10GB**のログになっているかと思いますのでこのブロックの最初から終わりまでが10GBのファイル転送にかかった時間になります。  

    - Sample
    ```
    2019-12-23 10:45:30.975 [6a0-00001350] LOG Alternate log directory: "C:/Users/user/AppData/Local/Aspera/Aspera Connect/var/log"
    2019-12-23 10:45:32.005 [6a0-00001350] LOG (access key) Not present
    |
    2019-12-23 10:49:59.276 [6a0-00001350] LOG Source bytes transferred                    : 10737418240
    2019-12-23 10:49:59.276 [6a0-00001350] LOG ======= end File Transfer statistics =======
    ```

    このログで、10GBのファイル転送に **4分30秒** ほどで完了していることが確認できます。  

## 初期設定値の変更  
Aspera Connectではインストール時にダウンロード先や転送速度など規定の値が設定されていますが設定を変更することもできます。  
今回はダウンロード先のフォルダの指定と転送時の最大帯域幅の設定を行います。  

### 手順
1. タスクトレイの中のAsperaのアイコンを右クリックし、**設定** を選択します。  

2. 設定のウインドウが表示されます。  *転送*タブを開きます。  

3. *ダウンロード*のセクションで **参照** をクリックします。

4. ダウンロード先を**デスクトップ**に指定し、**開く**をクリックします。  

5. **適用**をクリックし、*帯域幅*のタブをクリックします。  

6. ダウンロード、アップロード共にチェックを付け、**500MB**と指定します。  

7. 適用をクリックし、**ok**をクリックします。  

> 転送開始時の転送速度について  
> 各ダウンロード/アップロードの検証時は転送開始時の転送速度は**10Gbpsないし1Gbps**となっていましたが、帯域幅の設定を行うと、転送開始時の値が**10.00Mbps**に変更されます。  
> 速度が必要な場合は個別に設定をしてください。　　


以上です。Lab4では Asperaのクライアントソフトである Aspera Desktop Clientを利用してファイルのアップロード/ダウンロードを実行してみます。  



