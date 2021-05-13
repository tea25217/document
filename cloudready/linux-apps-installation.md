# CloudReadyでのLinuxアプリケーションインストール
CloudReadyのLinuxVMにアプリケーションをインストールする手順を都度書き残す。  
中身はDebian10(buster)のため、基本的にはDebianやUbuntuでの方法を探せばよい。  
<br>

- Firefox-esr  

~~~
sudo apt install firefox-esr-l10n-ja  
~~~
<br>

- FileZilla  

~~~  
sudo apt install filezilla  
~~~   
<br>

- wine  

下記記事をやる。  
(Debian。ディレクトリ構成の違いから「64bit版の検証」の節は確認できなかったがスルー。)  
<br>
Linux Mint・Ubuntu・Debian に wine 4.18 をインストールする  
https://dskjal.com/linux/install-wine.html  
<br>

- snap  

~~~
sudo apt update && sudo apt install snapd
~~~  
PC再起動後に以下。(VMだけ再起動でいい気もする)  
~~~
sudo apt install libsquashfuse0 squashfuse fuse
sudo snap install core
~~~  
<br>

- ModernDeck  

公式サイトからAppImageをダウンロードし適当なフォルダに格納、実行権限を付与。  
https://moderndeck.org/    

~~~
chmod a+x appimage/ModernDeck_x86_64.AppImage
~~~  

エイリアスを張る。

~~~
sudo nano ~/.bash_aliases
~~~  

以下を.bash_aliasesに追記。  

~~~
alias moderndeck='appimage/ModernDeck_x86_64.AppImage'
~~~
<br>

- Kindle  

前提条件:wineインストール済み  
<br>
下記記事後半の手順でインストールする。  
<br>
UbuntuでもKindle 本が読みたい  
https://text.baldanders.info/remark/2019/05/kindle-for-wine/  
<br>
ランチャーに追加されたアイコンでは起動できない(対処法不明)ため、コマンドラインで起動する。  
wineでexeを実行すればよいが、毎回パスを渡すのは面倒なのでエイリアスを張る。  

~~~
sudo nano ~/.bash_aliases
~~~  

以下を.bash_aliasesに追記。  

~~~
alias kindle='wine ~/.wine/drive_c/Program\ Files\ \(x86\)/Amazon/Kindle/Kindle.exe'
~~~
<br>

- VSCode  
https://code.visualstudio.com/blogs/2020/12/03/chromebook-get-started  
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

- IntelliJ IDEA  

前提条件:snap、JDKインストール済み  
<br>
~~~
sudo snap install intellij-idea-community --classic  
sudo chmod 755 /  
sudo chmod -R 755 /var/lib/snapd  
~~~  
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
本稿執筆時点でdefault-jdkがJava11の前提。  
<br>

- JavaFX  

https://gluonhq.com/products/javafx/  
https://openjfx.io/openjfx-docs/#install-javafx  
<br>
解凍したディレクトリを適当な場所へ格納し、  
環境変数$PATH_TO_FXを永続化しておく。  
~~~
sudo mv javafx-sdk-11.0.2 /usr/local/lib/  
sudo nano /etc/systemd/user/cros-garcon.service.d/cros-garcon-override.conf   
~~~
以下を追記する。  
~~~
Environment="PATH_TO_FX=/usr/local/lib/javafx-sdk-11.0.2/lib"
~~~

<details>  
<summary>(折り畳み)普通のLinux用</summary>
<br>
.profileに環境変数を設定。  
<br>
  
~~~
nano ~/.profile  
~~~
~~~
# here my settings
if [ -d "/usr/local/lib/javafx-sdk-11.0.2/lib" ] ; then
    PATH_TO_FX="/usr/local/lib/javafx-sdk-11.0.2/lib"
fi
~~~  
<br>    
―――折り畳みここまで―――  

</details>
<br>
  
実行が面倒なのでエイリアスを張っておく。  
~~~
nano ~/.bash_aliases  
~~~
以下の内容で.bash_aliasesを作成もしくは追記する。  
--add-modulesには使いそうなのを突っ込んでおく。  
https://openjfx.io/javadoc/11/  
~~~
if [ -n "$PATH_TO_FX" ] ; then
    alias javafx='java --module-path $PATH_TO_FX --add-modules=javafx.controls,javafx.fxml,javafx.web'
fi
~~~
<br>

- VisualVM  

公式からダウンロードしunzip後に適当な場所へ格納、エイリアスを張っておく。（スタンドアローン版）  
https://visualvm.github.io/download.html  
~~~
unzip visualvm_207.zip
mv visualvm_207 /usr/local/bin/
rm visualvm_207.zip
nano .bash_aliases
~~~  
~~~
alias visualvm='/usr/local/bin/visualvm_207/bin/visualvm'
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
