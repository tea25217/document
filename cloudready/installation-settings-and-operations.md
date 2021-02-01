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
.bash_profileを作っておく。  
~~~
nano ~/.bash_profile
~~~
以下の内容を書き保存。  
~~~
if [ -f ~/.profile ]; then
    . ~/.profile
fi

if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi

~~~
<br>
<br>


# Linuxアプリケーションインストール  
別ページに記載。  
https://github.com/tea25217/document/blob/main/cloudready/linux-apps-installation.md  
