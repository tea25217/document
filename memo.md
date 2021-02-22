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

- git  
環境：  
any  
事象：  
前回のコミットメッセージを再利用したい  
対処：  
~~~
git commit --reuse-message=HEAD
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
JavaFX系の依存モジュールを--add-modulesで取り込む。  
~~~
java --module-path $PATH_TO_FX --add-modules=javafx.controls,javafx.fxml,javafx.web -jar foobar.jar  
~~~
くそめんどい。  
<br>
じゃあエイリアスを張る。  
※必要になったものを--add-modulesに都度書き足す  
~~~
if [ -n "$PATH_TO_FX" ] ; then
    alias javafx='java --module-path $PATH_TO_FX --add-modules=javafx.controls,javafx.fxml,javafx.web'
else
    alias javafx='echo "unchi"'
fi
~~~  
<br>

- Java  
環境：  
any  
事象：  
IntelliJ IDEAでJavaFXを使う  
対処：  
下記記事。  
https://java.keicode.com/devenv/javafx11-intellij.php  
<br>

- Crostini  
環境：  
Linux/Debian(CloudReady/Crostini)  
事象：  
環境変数をどこに書くべきか。  
systemd配下の下記ファイルに存在する変数はオーバーライドされる。  
~~~
/etc/systemd/user/cros-garcon.service.d/cros-garcon-override.conf  
~~~
対処：  
cros-garcon-override.confに記載   
<br>
.profileに書いても変数名が重複しなければ機能するが、独自仕様に合わせるのが安全。記法も簡易。  
<br>
