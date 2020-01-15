# Lab5 Linuxクライアントを使ったAspera CLIによるファイル転送  
Aspera HST Serverとは別のLinux Client（CentOS）を使って、HST ServerとCLIベースでファイル転送を行います。  
このLabではすべてCLIで操作を完結させるためにCentOSへの接続もSSHで行います（GUIは使いません）

## Aspera CLIのインストール
Aspera CLIをダウンロードし利用できるように設定します。  

### 手順  
1. Hyper-V ホストのデスクトップ上にある **CentOS** のショートカットを実行します。
   Teratermが開きます。

2. 下記のコマンドを実行してファイルをダウンロードします。  
   `curl -O https://download.asperasoft.com/download/sw/cli/3.9.2/ibm-aspera-cli-3.9.2.1426.c59787a-linux-64-release.sh`
   <br>

   ```
   # curl -O https://download.asperasoft.com/download/sw/cli/3.9.2/ibm-aspera-cli-3.9.2.1426.c59787a-linux-64-release.sh
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                       Dload  Upload   Total   Spent    Left  Speed
      100 14.6M  100 14.6M    0     0  1339k      0  0:00:11  0:00:11 --:--:-- 2398k
   # ls -l
   ‡Œv 15012
   -rw-------. 1 root root     2080 12ŒŽ 13 20:12 anaconda-ks.cfg
   -rw-r--r--. 1 root root 15363012 12ŒŽ 23 14:40 ibm-aspera-cli-3.9.2.1426.c59787a-linux-64-release.sh
   -rw-r--r--. 1 root root     2113 12ŒŽ 13 11:17 initial-setup-ks.cfg
   ```

3. 下記コマンドを実行してダウンロードしたスクリプトを実行します。

   ```
   chmod 755 ibm-aspera-cli-3.9.2.1426.c59787a-linux-64-release.sh
   ./ibm-aspera-cli-3.9.2.1426.c59787a-linux-64-release.sh
   export PATH=/root/.aspera/cli/bin:$PATH
   export MANPATH=/root/.aspera/cli/share/man:$MANPATH
   ```

   ```bash:log
   # chmod 755 ibm-aspera-cli-3.9.2.1426.c59787a-linux-64-release.sh
   # ls -l
   ‡Œv 15012
   -rw-------. 1 root root     2080 12ŒŽ 13 20:12 anaconda-ks.cfg
   -rwxr-xr-x. 1 root root 15363012 12ŒŽ 23 14:40 ibm-aspera-cli-3.9.2.1426.c59787a-linux-64-release.sh
   -rw-r--r--. 1 root root     2113 12ŒŽ 13 11:17 initial-setup-ks.cfg
   # ./ibm-aspera-cli-3.9.2.1426.c59787a-linux-64-release.sh

   Installing IBM Aspera CLI

   Installation into /root/.aspera/cli successful

   Optional installation steps:

     To include aspera in your PATH, run this command (or add it to .bash_profile):
       export PATH=/root/.aspera/cli/bin:$PATH

     To install the man page, run the following command:
       export MANPATH=/root/.aspera/cli/share/man:$MANPATH

   # export PATH=/root/.aspera/cli/bin:$PATH
   # export MANPATH=/root/.aspera/cli/share/man:$MANPATH
   ```

4. CLIのライセンス、バージョンを表示し、コマンドが実行できるか確認します。  
   `ascp -A` および `ascp4 -A` が実行でき、値が返ってくるか確認します。  
   <br>

   ```
   [root@centos ~]# ascp -A
   IBM Aspera CLI version 3.9.2.1426.c59787a
   ascp version 3.9.1.168954
   Operating System: Linux
   FIPS 140-2-validated crypto ready to configure
   AES-NI Supported
   License max rate=(unlimited), account no.=1, license no.=2000

   [root@centos ~]# ascp4 -A
   ascp4 version 3.9.1.168954
   Operating System: Linux
   ```

5. Aspera HST Serverから**testfile_10GB**をダウンロードします。使用できる帯域は **1Gbps**に設定します。  
   - file: /testfile_10GB
   - port: 33001
   - 帯域: 1000000 (1Gbps)
   - user: aspuser01
   - password: aspera

   - 実行するコマンド
   ```
   export ASPERA_SCP_PASS=aspera
   ascp -P 33001 -l 1000000 aspuser01@asperaserver.local:/testfile_10GB ./
   ```

   - 結果  
   ```
   # export ASPERA_SCP_PASS=aspera
   # ascp -P 33001 -l 1000000 aspuser01@asperaserver.local:/testfile_10GB ./
   testfile_10GB                                 100%   10GB  576Mb/s    01:56
   Completed: 10485760K bytes transferred in 116 seconds
    (738472K bits/sec), in 1 file.
   ```

   実施した結果のかかった時間をメモしておきます。
   - ここでは、 116 seconds 

> aspuser01のパスワード指定
> 今回は環境変数 **ASPERA_SCP_PASS** にパスワードをセットすることで、ascpコマンド実行時にパスワードを入力することを省いています。 `export ASPERA_SCP_PASS=aspera` を実行せずにascpコマンドで転送を行うとパスワードの入力を求められます。

> デフォルトの転送目標速度  
> ascpを利用する場合はデフォルトの転送目標速度が **10Mbps**に固定されています。  
> 高速な転送を行う場合は オプション "-l" を使用し、目標速度を調整してください。  
> 100Mbps -> 100000
> 1Gbps ->   1000000
> Asperaでは使用できる帯域幅がライセンスで決まっています。ライセンスで規定されている速度以上の設定をしても上限値はライセンスの帯域幅になります。  


## scpによるファイル転送との時間を比較  
CLIベースでAsperaファイル転送を行いましたが、SCPと行った場合にどのくらいの違いが出るか確認します。  

### 手順  

1.下記コマンドを実行して、先ほどファイル転送したファイルを削除します。  
  `rm -rf testfile_10GB`

2. **testfile_10GB** をダウンロードします。  
   `scp root@asperaserver.local:/docroot/testfile_10GB ./`

   ```
   # scp root@asperaserver.local:/docroot/testfile_10GB ./
   The authenticity of host 'asperaserver.local (192.168.0.10)' can't be established.
   ECDSA key fingerprint is SHA256:8qsbDES9KHfcQLP6g0F/3xwf+AS3JVwip4ydBQ8+wOM.
   ECDSA key fingerprint is MD5:95:5b:78:53:c4:c6:40:9d:4f:0a:b1:9e:77:05:b6:3c.
   Are you sure you want to continue connecting (yes/no)? yes  # yesと入力しEnter
   Warning: Permanently added 'asperaserver.local,192.168.0.10' (ECDSA) to the list of known hosts.
   root@asperaserver.local's password:  # Asperaserver.localのrootのパスワードを入力  
   testfile_10GB                                                            100%   10GB  92.8MB/s   01:50
   ```

## パケットロスや遅延が発生している環境での比較  
おそらく、先ほどの作業ではAsperaとSCPでそれほど差がない、もしくはSCPのほうがはやいという結果になっているかと思います。
次に実際のケースを想定して、ネットワークの遅延やパケットロスを意図的に起こし、結果を確認します。

> Asperaの得意な分野はネットワークの品質に問題がある場合や遠隔地の場合になります。  
> マルチクラウドな環境で且つ海外とのやりとり、クラウドの海外リージョンとのやりとりなどで真価を発揮します。  
今回は2つのケースを想定して作業します。

- 国内DCやクラウドサービスの国内リージョンとファイル転送
  - ネットワーク遅延: 20ms
  - パケットロス率: 1%  
- 海外拠点やクラウドの海外リージョンとファイル転送
  - ネットワーク遅延: 100ms
  - パケットロス率: 3%  


### 手順  

#### 20ms遅延/1%パケットロス想定  
1. 意図的にネットワークに遅延とパケットロスを発生させます。
   `tc qdisc add dev eth0 root netem loss 1% delay 20ms`

   ```
   # tc qdisc add dev eth0 root netem loss 1% delay 20ms
   # tc qdisc show dev eth0
   qdisc netem 8004: root refcnt 65 limit 1000 delay 20.0ms loss 1%
   ```

2. 前の手順で使ったファイルが残っていれば削除します。  
   `rm -rf testfile_*`

3. SCPでファイルコピーをします。今回は時間短縮のために1GBのファイルで検証します。
   `scp root@asperaserver.local:/docroot/testfile_1GB ./`

   - 結果
   ```
   # scp root@asperaserver.local:/docroot/testfile_1GB ./
   root@asperaserver.local's password:
   testfile_1GB                                                             100% 1024MB  51.1MB/s   00:20
   ```

4. SCPで転送したファイルを削除し、Asperaでファイルを転送します。  

   ```
   rm -rf testfile_*
   export ASPERA_SCP_PASS=aspera
   ascp -P 33001 -l 1000000 aspuser01@asperaserver.local:/testfile_1GB ./
   ```
   
   - 結果  
   ```
   # ascp -P 33001 -l 1000000 aspuser01@asperaserver.local:/testfile_1GB ./
   testfile_1GB                                                             100% 1024MB  374Mb/s    00:14
   Completed: 1048576K bytes transferred in 14 seconds
    (588447K bits/sec), in 1 file.
   ```

#### 100ms遅延/3%パケットロス想定

1. 意図的にネットワークに遅延とパケットロスを発生させます。
   `tc qdisc del dev ens192 root`
     - 前回の設定を削除します。

   `tc qdisc add dev eth0 root netem loss 3% delay 100ms`

   <br>

   ```
   # tc qdisc add dev eth0 root netem loss 3% delay 100ms
   # tc qdisc show dev eth0
   qdisc netem 8003: root refcnt 65 limit 1000 delay 100.0ms loss 3%
   ```

2. 前の手順で使ったファイルが残っていれば削除します。  
   `rm -rf testfile_*`

3. SCPでファイルコピーをします。今回は時間短縮のために1GBのファイルで検証します。
   `scp root@asperaserver.local:/docroot/testfile_1GB ./`

   - 結果
   ```
   # scp root@asperaserver.local:/docroot/testfile_1GB ./
   root@asperaserver.local's password:
   testfile_1GB                                                             100% 1024MB   9.5MB/s   01:47
   ```

4. SCPで転送したファイルを削除し、Asperaでファイルを転送します。  

   ```
   rm -rf testfile_1GB
   export ASPERA_SCP_PASS=aspera
   ascp -P 33001 -l 1000000 aspuser01@asperaserver.local:/testfile_1GB ./
   ```
   
   - 結果  
   ```
   # ascp -P 33001 -l 1000000 aspuser01@asperaserver.local:/testfile_1GB ./
   testfile_1GB                                                             100% 1024MB  464Mb/s    00:13
   Completed: 1048576K bytes transferred in 13 seconds
   (628900K bits/sec), in 1 file.
   ```

ネットワーク遅延がなく、安定しているネットワーク環境間（同一DCなど）であればSCPなどでも問題ありませんが、遠隔地且つパケットロスが発生するかもしれないネットワークになればなるほどAsperaを使うことでより安定したサービスを提供できるようになります。
また、どちらのネットワークのケースであってもAsperaの転送速度は変わらず、安定した転送速度を保つことができます。

- ネットワーク遅延などを発生しない環境で1GBファイルをAsperaで転送した結果

```
# ascp -P 33001 -l 1000000 aspuser01@asperaserver.local:/testfile_1GB ./
testfile_1GB                                                             100% 1024MB  667Mb/s    00:13
Completed: 1048576K bytes transferred in 13 seconds
 (648806K bits/sec), in 1 file.
```


以上でLab5は終わりです。次のLab6では Aspera Consoleをインストールし、Aspera HST Serverを管理します。  

