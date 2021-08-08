# MySQL 學習筆記(一) - 在了解資料庫前，先徹底了解資料庫是什麼?如何運作?它與SQL的關係是什麼? 



## 1. 什麼是資料庫?

核心定義

+ Collection Data: 數據集合

+ Methods for accessing and manipulating that data: 存取和操作數據的方法

  

**為什麼要存取或操作數據?** 

舉例: 因為如果手邊有非常大量的客戶數據，我們今天要查詢Jack的顧客資訊，我們不可能一個一個找，所以就要對數據進行訪問和操作





## 2. 資料庫的操作流程

![image1](images\image1.jpg)



+ 透過應用程式與DBMS(資料庫管理系統)進行溝通，然後DBMS會幫助我們與Database進行溝通並存取與操作數據，並一路傳回我們所需的數據

+ 通常會把整個DBMS -> Database稱為Database



## 3. SQL是什麼?





+　SQL全名為Structured Query Language，直翻就是結構化的查詢語言
+　用來和資料庫Database進行溝通，根據ANSI(American National Standards Institute)，它是關聯式資料庫管理系統(Relational Database management Systems)所使用的標準語言
+　簡單來說，它就是我們與資料庫間進行溝通的語言，包括資料插入、查詢、更新和刪除，資料庫模式的建立和修改，和數據的存取控制
+　以下圖來說，SQL在整個資料庫的操作流程會在App與整個Database之間進行溝通的語言



**觀念: SQL是使用者對關聯式資料庫進行溝通的語言**

![image2](images\image2.jpg)





## 4. 為什麼要選擇MySQL?



MySQL開源而且它的使用率排名根調查位居第二，第一的Oracle是需要收費的

參考網址: https://db-engines.com/en/ranking

![image3](images\image3.PNG)





## 5. SQL與MySQL的關係?



剛剛我們已經討論過SQL是什麼了，接著我們聊聊MySQL跟SQL的關係

| SQL                                                          | MySQL                           |
| ------------------------------------------------------------ | ------------------------------- |
| 為一種用來操作關聯式資料庫系統的語言                         | 為一種關聯式資料庫管理系統RDBMS |
| 用來對資料庫進行資料插入、查詢、更新和刪除，資料庫模式的建立和修改，和數據的存取控制 | 為開源資料庫                    |
| 結構化查詢語言                                               | 一種關聯式資料庫管理系統的軟體  |

![image4](images\image4.jpg)



圖片說明了，SQL是我們用來對關聯式資料庫管理系統進行操作的語言，而MySQL就是關聯式資料庫管理系統的其中一個代表軟體，當然也包括SQLite、Oracle等





**補充:**  NoSQL與其對應的非傳統關聯式資料庫系統



**重要觀念:**

+ 我非常喜歡WIKI上所表達的NoSQL，過去表示的是Non-SQL也就是應用於非關聯式資料庫管理系統的統稱，但現在被轉解成Not only SQL，我的理解跟我查詢到的相關資料，就是指現在的NoSQL不能說它一定就是非關聯式資料庫管理系統的語言，因為它可能還是具有一些關聯，所以這個定義: NoSQL是應用於**非傳統**關聯式資料庫系統的統稱，就表達的非常的好
+ 雖然NoSQL應用於非關聯式資料庫，但是不能說一定沒有關聯，而只是這種關聯是用另外的形式來表達呈現





參考網址：　https://zh.wikipedia.org/wiki/NoSQL

![image5](images\image5.jpg)

圖片說明了，MySQL是我們用來對**非傳統**關聯式資料庫系統的統稱，而mongoDB就是關聯式資料庫管理系統的其中一個代表軟體，當然也包括CouchDB等







如果大家想知道更多關於SQL與NoSQL的差別，可以參考這篇(https://read01.com/GPnEx.html#.YQ4zgYgzaUk)，我覺得寫得很詳細，包括他們的優勢、劣勢、使用原因等等





## Reference

https://kuochingsouthen.com/what-is-sql/

https://zh.wikipedia.org/wiki/SQL

https://zh.wikipedia.org/wiki/NoSQL

https://read01.com/GPnEx.html#.YQ4zgYgzaUk

