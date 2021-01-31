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
環境変数をどこに書くか  
対処：  
システム理解が深まるまで保留。  
普通のLinuxと違い、以下のファイルに存在する変数は上書きされる。
~~~
/etc/systemd/user/cros-garcon.service.d/cros-garcon-override.conf  
~~~
<br>
