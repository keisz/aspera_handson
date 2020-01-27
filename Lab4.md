# Lab4 Windows クライアントにAspera Desktopの導入とアップロード/ダウンロード試験  
Lab3ではAsperaのWebUIを利用し、Aspera Connect経由でファイルの転送を行いました。Lab4ではクライアントソフトウェアであるAspera Desktop Clientを利用してAspera HST Serverとファイル転送を行います。  
作業概要は下記になります。

- Aspera Desktop Clientのインストール
- ファイル転送の試験、帯域の変更

## Aspera Desktop Clientのインストール  
Hyper-V上に用意されているWindows 10を利用し、Aspera Desktopをインストールします。  

### 手順  

1. デスクトップ上のリンクからFirefoxを起動します。

2. **Aspera desktop client windows** と入力し、Enterを押打します。  

3. 検索結果で **aspera client - Aspera Downloads **のエントリがでますので、クリックします。  
   - URL: https://downloads.asperasoft.com/en/downloads/2
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/60-1.png)

4. ドロップダウンリストから **v3.9.3 - Windows** を選択します。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/61-1.png)

5. **Download**ボタンをクリックします。 ポップアップが表示されますので、**ファイルに保存**を選択し、ダウンロードします。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/62-1.png)

6. Firefoxのダウンロードファイル一覧からダウンロードしたファイルをクリックし、実行します。
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/63-1.png)

7. UACのセキュリティポップアップが表示されますので、*はい*を選択します。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/64-1.png)

8. セットアップウィザードが表示されます。**次へ**をクリックします。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/65-1.png)

8. ライセンスの許諾が表示されます。**同意します**を選択し、**次へ**をクリックします。
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/66-1.png)

9. 標準を選択します。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/67-1.png)

10. Asperaサービスアカウントを作成します。ユーザー名は変えずに、パスワードだけ入力し、**次へ**をクリックします。  
    - password: Aspera123! </br>
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/69-1.png)

10. **Confirm Create User** が表示されます。**はい**を選択します。
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/68-1.png)

11. ユーザー作成の警告が表示されます。 **はい**を選択します。  

12. インストールをクリックします。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/70-1.png)

13. **完了**をクリックします。 </br>
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/71-1.png)

14. Aspera Desktop Clientが表示されます。 **接続**をクリックします。 
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/74-1.png)

15. *接続*タブで左上の＋ボタンを押打し、ホスト、ユーザー、パスワードを指定します。  
    - ホスト: asperaserver.local
    - ユーザー: aspuser01
    - パスワード: aspera  <br>
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/73-1.png)

   設定後、**接続のテスト**をクリックします。 **サーバーに正常にアクセスしました**を表示されることを確認し、**OK**をクリックします。   

16. Desktop Clientの右カラムに **aspuser01@asperaserver.local** のエントリが追加されていますのでダブルクリックします。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/74-1.png)

17. Aspera HST Serverの */docroot* のファイルが表示されます。フォルダーのアイコンをクリックし、新しいフォルダーを作ります。今回は **Desktop_client_test**と作っています。 作成したフォルダーをダブルクリックし移動します。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/76-1.png)

18. 左側のカラムで デスクトップ(C:\users\user\desktop)に移動し、**テストファイル1.txt** を選択し、**[▶]** をクリックし転送します。
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/77-1.png)

19. 転送中、完了後のステータスは下部の転送タブから確認できます。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/78-1.png)

## ファイル転送の試験及び帯域の変更  
ファイル転送（アップロード）の試験と使用する帯域の設定について確認します。  

### 手順  
1. Desktop Clientで左カラムをデスクトップ、右カラムをAspera HST Serverの先ほど作成したフォルダーを表示します。  

2. **testfile_1GB**を選択し、**[▶]** をクリックし転送します。

3. 転送タブの速度を確認すると、速度が **45Mbps** 程度で止まっています。
   時間がかかりますので、転送タブで対象のエントリを選択し **[■]** で一時停止し、**[×]** で一度エントリを削除します。  

4. 右上の**設定**をクリックします。転送のステータスが表示されます。  
   デフォルト目標速度を ダウンロード、アップロードともに **1Gbps** に変更し、OKをクリックします。  
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/79-1.png)


5. 再度、**testfile_1GB**を選択し、**[▶]** をクリックし転送します。転送速度が 45Mbps以上（画像では920Mbps)になり、高速に転送されていることが確認できます。  

## ファイル/フォルダ名の変更  
Aspera HST Server上にファイルやフォルダに対しても操作が可能なことを確認します。
ここでは、**速度検証**フォルダのフォルダ名の変更と **テストファイル1.txt**の試してみます。

### 手順  
1. Desktop Clientで左カラムをデスクトップ、右カラムをAspera HST Serverのルートフォルダーを表示します。  

2. **速度検証**フォルダを右クリックし、**名前の変更**を選択します。
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/73-2.png)
   
3. フォルダ名を **speedtest** に変更し、Enterで確定します。

4. **テストファイル1.txt** を選択し右クリックし、削除します。
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/74-2.png)
   
5. HST Serverのリストから変更および削除されていることを確認します。
   ![](https://github.com/keisz/aspera_handson/blob/master/images/Lab4/75-2.png)


以上でLab4は終了になります。 次の[Lab5](https://github.com/keisz/aspera_handson/blob/master/Lab5.md) では、Linuxサーバー（CentOS)を使ってCLIベースでAsperaのファイル転送を行います。  


