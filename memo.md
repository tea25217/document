- git  
環境：  
Linux/Debian(CloudReady)  
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
Linux/Debian(CloudReady)  
事象：  
jarの実行  
対処：  
~~~
java -jar hoge.jar
~~~
<br>
binfmt-supportを入れると直接実行できるけど、chmod u+x hoge.jarする必要があってjarを作るたびにやるのめんどいからremoveした。  
<br>
