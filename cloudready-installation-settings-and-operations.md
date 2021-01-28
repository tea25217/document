愚かにも短期間で3回繰り返してしまったため、CloudReadyの環境構築手順と設定を書き残しておく。
<br>
<br>
# 背景
16GB USBメモリで試用  
↓  
30GB SSDにインストール  
↓  
容量不足で256GB SSDに換装  
<br>
ブラウザ上でも一通りの事はこなせるが、Linuxマシンとして使い倒すのであれば当然相応のディスク容量が必要になる。  
<br>
<br>
# 断念したこと
ディスク丸々クローンもしくは中で動いているLinuxVMをクローンできれば次回は本ドキュメントの手順を踏まなくてよい。  
<br>
### ディスククローン
クローンだけでは起動せず。  
ブートローダを修復するか、それで駄目なら下記URLを参考にやれば成功するかもしれない。  
<br>
Chrome OSのディスクをサイズアップする - adsaria mood  
https://adsaria.hatenadiary.org/entry/20091127/1259310143  
<br>
### Linux VMクローン
エクスポートは可能だが、インポートもしくはマウントができず断念。  
<br>
下記手順でexport→mountを試行。適切なマウント先が不明。  
<br>
Running Custom Containers Under Chrome OS  
https://chromium.googlesource.com/chromiumos/docs/+/master/containers_and_vms.md#Extracting-Disk-Images
<br>
<br>
>Note that there isn't yet a way to import a VM
<br>
importの方法を調べたが、まだ無い模様。
<br>
https://support.google.com/chromebook/thread/82740941?hl=en  

https://chromium.googlesource.com/chromiumos/docs/+/master/containers_and_vms.md#how-can-i-backup-a-vm  
<br>
<br>

# CloudReadyインストール
公式サイトからインストーラをダウンロードする。  
既存のCloudReady環境が生きているならば外付けドライブにchromeos-installでもよい。  
既存環境からコマンドを叩く場合は、CTRL+ALT+F2→chronosログイン→下記。  

~~~
chronos@localhost ~ $ /usr/sbin/chromeos-install --dst /dev/sdX
~~~
※sdXのXは適宜置き換え。内蔵ドライブが1つならリムーバブルディスクはsdbだが、df -i等で確認すること。  

以下参考URL。  
<br>
Chrome OS (Chromium OS)を触ってみた  
https://gato.intaa.net/archives/12914#  
<br>
<br>

# Linuxインストール
起動後、  
画面右下の時計をクリック→右上の歯車アイコンをクリック→設定画面左下「Linux(ベータ版)」→「オンにする」  
<br>
ディスクサイズは後から変更できないため、本体側で使いそうなギリギリを残して沢山割り当てる。  
<br>
<br>

# Linux初期設定
下記記事の3～4をやる。  
<br>
CloudReady上でLinuxアプリケーションを動作させよう！ 〜Chrome OS オープンソース版CloudReady stable 83.4.27 上のLinux環境構築と日本語化・・  
https://www.linux-setting.tokyo/2020/09/cloudready-stable-83427-chrome-os.html  
<br>
<br>

# Linuxアプリケーションインストール
使うものを入れる。都度編集。  
<br>
- Firefox-esr  
~~~
sudo apt install firefox-esr  
~~~

設定画面で日本語に変える一手間があるため、日本語版を指定してインストールする方が賢い。
<br>

- code-server  
~~~
curl -fsSL https://code-server.dev/install.sh | sh  
sudo systemctl enable --now code-server@$USER  
~~~

dockerコンテナで使用することも考えたが、アクセス制約やリソース圧迫が面倒で直接インストールにした。  
CTRL+@でターミナルを開くショートカットキーが機能しないため、  
Keyboard Shortcuts→Toggle Integrated TerminalのキーバインドをAlt+@に変更する。  
(設定画面の表示はバグってるが、アプリ再起動したら設定したキーで動作した)  
~~拡張機能は母艦PCのextensionsフォルダをコピーすると楽。~~ ←機能しなかった  
日本語化は下記URLの手順で。  

<br>
Visual Studio Codeの日本語化設定  
https://rfs.jp/sb/vsc/vsc-japanese.html  
<br>
まっさらなGoogleアカウントにインストールする場合はPWAに設定もする。  
<br>
Chromebookには、VS Code より code-server がオススメ  

https://note.com/digzero/n/n70ee0402a92b#9Zgsw  
<br>

