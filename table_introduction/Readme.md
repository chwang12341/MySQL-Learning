# MySQL 學習筆記(四) - MySQL中的資料類型 Data Type - 如何創建tables資料表?如何操作資料表? - 快速為自己創建一個資料表







## 1. 什麼是table資料表?





+　又被稱為表格，它就是我們用來存儲資料的地方
+　可以把它當成特定主題資料的集合，像是我們想要蒐集學生資料，那我們就會創建一個資料表，裡面可能有姓名、性別、成績等等，這個資料表中的資料都會與學生(主題)相關
+　結構由行和列所組成的二維表格
+　在關聯式資料庫中，table是用來代表存儲資料之間的關係
+　關聯式資料庫中，使用主鍵和外鍵來將資料表之間的關系串聯起來

更多關於關聯式資料庫得主成有哪些，名詞解釋可以參考https://www.ithome.com.tw/node/46156，我覺得寫得非常詳細，也可以參考http://cc.cust.edu.tw/~ccchen/doc/db_02.pdf，我覺得講者非常厲害，用很多的圖片帶大家了解關聯式資料庫



**示意圖**: 為了讓大家更能感受資料表是什麼，其實簡單來說，就是平常我們會用excel來記錄我們的一些資料，像是每月開銷或是工作項目等等，而我們所使用的excel概念，就是資料表table，但在SQL中我們要去指定每個欄位的資料類型，像是姓名就會是字符串類型，成績可能就是整數，性別也是字符串類型等等

![image1](images\image1.png)



![image2](images\image2.png)









## 2. MySQL中的資料類型 Data Type有哪些?

**觀念:** 當我們在創建資料表table時，我們會需要去指定每個新創見出來的欄位其資料型態是什麼，也可以根據欄位的實際狀況去限制這個欄位中的資料最大的字符數量，像是我們指定姓名為字符串類型中的VARCHAR，並限制其自符最長只能到20個字符 - VARCHAR(20)

![image3](images\image3.jpg)





## 3. SQL創建Table語法



+ 創建table的語法

```
CREATE TABLE <table_name>
(
	<column_name> <data_type>,
	<column_name> <data_type>,	
	<column_name> <data_type>,
	<column_name> <data_type>,
	...	
)
```



創建table

+ table_name: 設定table名稱

當我們要創建列的時候

+ column_name: 設定欄位名稱
+ data_type: 設定欄位的資料類型

說明: 資料類型根據欄位有不同的資料類型，像是如果是年齡欄位，那它的資料類型就可以設為INT，如果是性別欄為，資料類型就可以設為ENUM，如果是姓名欄位，就可以設為VARCHAR等等



舉例：當我們要創建一個學生資料表，我們可能需要姓名、年齡、生理性別欄位，然後我們要分別指定其資料型態

+ 姓名: 用字符串類型中的VARCHAR來指定，並規定最長只能到20個字符VARCHAR(20)
+ 年齡: 用數值類型中的整數INT來指定
+ 生理性別: 用字符串類型中的ENUM來指定，而生理性別會有男生跟女生的選項，所以會寫成ENUM('M','F)

```
mysql> CREATE table student(
    -> name VARCHAR(20),
    -> age INT,
    -> gender ENUM('M', 'F')
    -> );
Query OK, 0 rows affected (0.04 sec)
```



## 4. 操作Table的語句

| 語句                              | 說明                                        |
| --------------------------------- | ------------------------------------------- |
| show tables;                      | 顯示database中有哪些table                   |
| show columns from `<table_name>`; | 顯示table中以哪些列                         |
| desc `<table_name>;`              | 顯示table中以哪些列(與上面語句所呈現的一樣) |
| drop table `<table_name>;`        | 刪除掉table                                 |





## 5. 實作 - 創建一個學生資料表



+ 創建一個Database用來做我們這次的實作，取名為demo_student

```
mysql> create database demo_student;
Query OK, 1 row affected (0.00 sec)
```





+　切換到實作用的Database

```
mysql> use demo_student;
Database changed
```





+ 創建一個table，並取名為student

```
mysql> CREATE table student(
    -> student_id INT,
    -> name VARCHAR(40),
    -> gender ENUM("M", "F"),
    -> admission_date DATE
    -> );
Query OK, 0 rows affected (0.04 sec)
```



+ 使用desc來顯示剛剛創建的student資料表

```
mysql> desc student;
+----------------+---------------+------+-----+---------+-------+
| Field          | Type          | Null | Key | Default | Extra |
+----------------+---------------+------+-----+---------+-------+
| student_id     | int(11)       | YES  |     | NULL    |       |
| name           | varchar(40)   | YES  |     | NULL    |       |
| gender         | enum('M','F') | YES  |     | NULL    |       |
| admission_date | date          | YES  |     | NULL    |       |
+----------------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```



+ 使用show columns from student來顯示student資料表

```
mysql> show columns from student;
+----------------+---------------+------+-----+---------+-------+
| Field          | Type          | Null | Key | Default | Extra |
+----------------+---------------+------+-----+---------+-------+
| student_id     | int(11)       | YES  |     | NULL    |       |
| name           | varchar(40)   | YES  |     | NULL    |       |
| gender         | enum('M','F') | YES  |     | NULL    |       |
| admission_date | date          | YES  |     | NULL    |       |
+----------------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```



+ 使用show tables; 查看當前Database中有哪些table

```
mysql> show tables;
+------------------------+
| Tables_in_demo_student |
+------------------------+
| student                |
+------------------------+
1 row in set (0.00 sec)
```



+ 刪除掉student資料表

```
mysql> drop table student;
Query OK, 0 rows affected (0.01 sec)
```





+　查詢當前Database中有哪些table

```
mysql> show tables;
Empty set (0.00 sec)
```







## Reference 

https://dev.mysql.com/doc/refman/8.0/en/data-types.html

https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E5%BA%93%E8%A1%A8

https://www.runoob.com/mysql/mysql-data-types.html

http://cc.cust.edu.tw/~ccchen/doc/db_02.pdf

https://www.w3schools.com/sql/sql_datatypes.asp



