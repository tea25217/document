- git  
環境：  
Linux/Debian(CloudReady/Crostini)  
事象：  
pullしたものを実行したらパーミッションエラー  
対処：  
プロジェクトディレクトリごと所有権を変更する   
~~~
chown -R myname:myname /project/directory/
~~~
<br>

- Java  
環境：  
Linux/Debian(CloudReady/Crostini)  
事象：  
jarの実行  
対処：  
~~~
java -jar hoge.jar
~~~
binfmt-supportを入れると直接実行できるけど、chmod u+x hoge.jarする必要があってjarを作るたびにやるのめんどい。  
<br>

- Java  
環境：  
Linux/Debian(CloudReady/Crostini)  
事象：  
JavaFXを使ったjarの実行  
対処：  
~~~
java --module-path $PATH_TO_FX --add-modules javafx.controls -jar foobar.jar  
~~~
くそめんどい。  
<br>
じゃあエイリアスを張る。  
~~~
if [ -n "$PATH_TO_FX" ] ; then
    alias javafx='java --module-path $PATH_TO_FX --add-modules javafx.controls'
else
    alias javafx='echo "unchi"'
fi
~~~  
<br>

- Crostini  
環境：  
Linux/Debian(CloudReady/Crostini)  
事象：  
環境変数をどこに書くべきか。  
普通のLinuxと違い、下記ファイルに存在する変数はオーバーライドされる。  
~~~
/etc/systemd/user/cros-garcon.service.d/cros-garcon-override.conf  
~~~
対処：  
なるべくcros-garcon-override.confに記載、  
ユーザーごとに分ける必要がある場合のみ.profileに。  
<br>
ユーザー単位のものは.profileにとも思うが、  
上述の仕様に起因する混乱の抑止と、記法が簡易なことから、どうしても必要な場合を除いてなるべく  
cros-garcon-override.confに記載する方が良いだろうと一旦の結論。  
<br>
