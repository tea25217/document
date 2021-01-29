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
ブラウザ上でこなせる用途に割り切ればUSBメモリでも十分だが、  
Linuxマシンとして使い倒すのであれば当然相応のディスク容量が必要になる。  
<br>
<br>
# 断念したこと
設定→Linux(ベータ版)→Linux→「バックアップと復元」が上手くいくか、ディスク丸々クローンするか、  
もしくは中で動いているLinux VMをクローンできれば次回は本ドキュメントの手順を踏まなくてよい。  
<br>
### バックアップと復元
試しにバックアップしたら途中で固まってVMが起動しなくなったので、以後触っていない。  

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
<br>
Installing CloudReady  
https://www.neverware.com/freedownload#home-edition-install  
<br>
既存のCloudReady環境が生きているならば外付けドライブにchromeos-installでもよい。  
既存環境からコマンドを叩く場合は、CTRL+ALT+F2→chronosログイン→下記。  

~~~
/usr/sbin/chromeos-install --dst /dev/sdX
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
**ディスクサイズは後から変更できない**ため、本体側で使いそうなギリギリを残して沢山割り当てる。  
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
日本語化の方法はVSCodeと同じ。  
<br>
Visual Studio Codeの日本語化設定  
https://rfs.jp/sb/vsc/vsc-japanese.html  
<br>
PWAへの設定も行う。  
(旧環境で設定済みのGoogleアカウントならば引き継がれているため不要)  
<br>
Chromebookには、VS Code より code-server がオススメ  
https://note.com/digzero/n/n70ee0402a92b#9Zgsw  
<br>

- git  

旧バージョンが入っているため、アンインストール後に最新版をインストールする。  
<br>
最新版の Git をインストールする  
https://qiita.com/noraworld/items/8546c44d1ec6d739493f  
<br>
初期設定を済ませる。  
~~~
git config --global user.name "My display name"  
git config --global user.email "mymail@foobar.com"  
ssh-keygen  
cat /.ssh/id_rsa.pub
~~~
吐き出された公開鍵をssh-rsa～から全部コピペしてgithubに追加。  
<br>
SSH keys  
https://github.com/settings/keys  
<br>
- JDK  
~~~
sudo apt install default-jdk  
java -version  
javac -version  
~~~
<br>

- JavaFX  

https://gluonhq.com/products/javafx/  
https://openjfx.io/openjfx-docs/#install-javafx  
<br>
環境変数$PATH_TO_FXを永続化しておく。  
~~~
nano ./.profile  
~~~
末尾に以下を追加する。  
~~~
~~~
<br>

- sbt  

Linux への sbt のインストール  
https://www.scala-sbt.org/1.x/docs/ja/Installing-sbt-on-Linux.html#Ubuntu+%E5%8F%8A%E3%81%B3%E3%81%9D%E3%81%AE%E4%BB%96%E3%81%AE+Debian+%E3%83%99%E3%83%BC%E3%82%B9%E3%81%AE+Linux+%E3%83%87%E3%82%A3%E3%82%B9%E3%83%88%E3%83%AA%E3%83%93%E3%83%A5%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3  
<br>

- Python  

3.9のインストールと、デフォルトを変更。  
~~~
sudo apt install wget build-essential libreadline-gplv2-dev libncursesw5-dev \
     libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev  
wget https://www.python.org/ftp/python/3.9.1/Python-3.9.1.tgz  
tar xzf Python-3.9.1.tgz  
cd Python-3.9.1  
./configure --enable-optimizations  
sudo make altinstall  
sudo update-alternatives --install /usr/bin/python python /usr/local/bin/python3.9.0 0  
python -V
~~~
最新バージョンの確認先と参考URL。  
<br>
Index of /ftp/python/  
https://www.python.org/ftp/python/  
<br>
How To Install Python 3.9 on Debian 10  
https://tecadmin.net/how-to-install-python-3-9-on-debian-10/  
<br>
