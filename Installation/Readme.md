# MySQL 學習筆記(二) - 一分鐘輕鬆瞭解如何在Windows上安裝MySQL



哈囉，大家好!! 介紹完SQL的概念，接下來帶大家在Windows上一起安裝MySQL



## 步驟一

要找企鵝要去北極，要找北極熊就要去南極(是這樣嗎XD)，所以要安裝MySQL當然是到官網安裝起來啦

到官網後，點擊Downloads

![image1](images\image1.png)





## 步驟二

到最下面會看到一個 MySQL Community (GPL) Downloads，點擊它

![image2](images\image2.png)



## 步驟三

由於小弟我的電腦是Windows，所以我就點選 MySQL Installer for Windows

![image3](images\image3.png)



## 步驟四

目前最新的版本是8.0.26，而為了公司需求，所以我要安裝以前的版本，我就點選旁邊Looking for previous GA versions? 當然大家可以根據自己需求安裝最香的版本

![image4](images\image4.png)



## 步驟五

點進去後，會是5.7.35版本，而我選擇的是下面的community版，而不是上面的Web community版，當然它也會比較占我們的電腦空間，點擊Download

![image5](images\image5.png)

![image6](images\image6.png)



## 步驟六 - 開始進入到我們的安裝

點擊好後，它就會下載一個mysql-installer到我們的電腦，接著直接點擊它，接下來就是一連串的Next跟Execute



選擇預設的Developer Default，按下Next

![image7](images\image7.png)





![image8](images\image8.png)



下面出現了 需多MySQL需要安裝的檔案，給它按Execute，它就會自動幫大家安裝了喔

![image9](images\image9.png)

這需要一點時間，等起來

![image10](images\image10.png)



大家這邊不用改Port喔，就用預設的3306，除非Port 3306已經被佔用了，如果被佔用可以使用`netstat -aon|findstr 3306`指令來看誰佔用了Port 3306(找到PID)，到開啟工作管理員把它(根據PID)關掉，就可以囉，接下來繼續按Next

![image11](images\image11.png)



![image12](images\image12.png)

大家來為自己的MySQL設定一組密碼，後續連接MySQL的時候會用到

![image13](images\image13.png)



按下check後，會顯示連結成功，就可以按Next了

![image14](images\image14.png)

按下Execute

![image15](images\image15.PNG)

![image16](images\image16.PNG)

安裝的最後一步!!

![image17](images\image17.PNG)





## 步驟七 - 安裝好後確認是否有連接成功

打開MySQL5.7 Command Line Client

傳入大家剛剛設定好的root密碼

![image18](images\image18.PNG)





接著打上`\help`來看一下一些指令

![image19](images\image19.PNG)



接著要確定我們有沒有連接上MySQL Database，就要寫進`show databases;`來看，如果有顯示，就表示大家成功了喔

![image20](images\image20.PNG)









大功告成!!辛苦大家了!!我們又朝MySQL學習之路邁進一大步了





