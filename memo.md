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

- JAVA  
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

- JAVA  
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

