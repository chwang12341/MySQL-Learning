# MySQL 學習筆記(五) - 如何將數據插入Table? 如何限制填入的數據必須有值? 如何設定沒有填入數據的狀況下預設值是什麼? Insert, Select, NULL/NOT NULL語句用法



## 1. Insert 用法 - 如何將數據插入? 



**語法**: insert into `table name`(`column1 name`, `column2 name`, `column3 name`...) values (`column1 value`, `column2 value`, `column3 value` ...)

```
insert into <table name>(<column1 name>, <column2 name>, <column3 name>...) values (<column1 value>, <column2 value>, <column3 value> ...)
```



**實作**

+ 我們先快速地創建一個database名稱為demo，接著使用這個資料庫，然後創建一個table(這部份是上一篇筆記(四)所提到的內容，我這邊就不多做解釋)

```sql
mysql> create database demo;
Query OK, 1 row affected (0.00 sec)

mysql> use demo;
Database changed
mysql> CREATE table student(
    -> student_id INT,
    -> name VARCHAR(40),
    -> gender ENUM("M", "F"),
    -> admission_date DATE
    -> );
Query OK, 0 rows affected (0.04 sec)
```



+ 接著我們插入如圖表的資訊進到我們的table裡面

![image1](images\image1.png)

```sql
insert into student(student_id, name, gender, admission_date) values (1, "Jack", "M", "2021-08-21" );
```



+　如何插入更多的數據進到我們的table，如圖表

![image2](images\image2.png)



**方法一: 一直重複輸入上面的語法方法**

```sql
mysql> insert into student(student_id, name, gender, admission_date) values (1, "Jack", "M", "2021-08-21" );
Query OK, 1 row affected (0.00 sec)

mysql> insert into student(student_id, name, gender, admission_date) values (2, "Tom", "M", "2020-08-21" );
Query OK, 1 row affected (0.00 sec)

mysql> insert into student(student_id, name, gender, admission_date) values (3, "Cindy", "F", "2019-08-21" );
Query OK, 1 row affected (0.00 sec)
```



**方法二: 一次全部輸入的方法**

```sql
mysql> insert into student(student_id, name, gender, admission_date) values (1, "Jack", "M", "2021-08-21" ), (2, "Tom", "M", "2020-08-21" ), (3, "Cindy", "F", "2019-08-21" );
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0
```







## 2. Select用法 - 如何決定顯示哪些table列的數據?



**語法:**

**全部列都顯示**: SELECT * from `table name`

```sql
mysql> SELECT * from student;
+------------+-------+--------+----------------+
| student_id | name  | gender | admission_date |
+------------+-------+--------+----------------+
|          1 | Jack  | M      | 2021-08-21     |
|          2 | Tom   | M      | 2020-08-21     |
|          3 | Cindy | F      | 2019-08-21     |
+------------+-------+--------+----------------+
3 rows in set (0.00 sec)
```





**只顯示部份列**

+ 只顯示某一列: SELECT `column name` from `table name`

ex. 只顯示name那一列

```sql
mysql> SELECT name from student;
+-------+
| name  |
+-------+
| Jack  |
| Tom   |
| Cindy |
+-------+
3 rows in set (0.00 sec)
```





+　指定顯示哪些列: SELECT `column1 name`, `column2 name`, ... from `table name`

ex. 我想只顯示id、gender跟admission_date

```sql
mysql> SELECT student_id, gender, admission_date from student;
+------------+--------+----------------+
| student_id | gender | admission_date |
+------------+--------+----------------+
|          1 | M      | 2021-08-21     |
|          2 | M      | 2020-08-21     |
|          3 | F      | 2019-08-21     |
+------------+--------+----------------+
3 rows in set (0.00 sec)
```





## 3. NULL / NOT NULL 用法 - 如何限制數據在輸入時，哪些列是可以不填，哪些列是一定要填入的



+　顯示我們剛剛創建的Table

```sql
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



![image3](images\image3.png)



大家會注意到除了列名、資料型態，後面其實還有一些是我們可以設定的，我們這邊要看到的是NULL這個欄位，下面都為YES，表示當我們在傳入數據時，這個欄位是可以為空的，也就是可以省略不輸入的，而在不輸入的情況下，MySQL會使用後面那個Default欄位裡面的值來填入，也就是我們的預設值



ex. 假設我們要輸入一筆數據，但我們只有姓名跟入學日期，這樣輸入進去的話，其它的資訊就會被填為NULL

```sql
mysql> insert into student(name, admission_date) values ('Ken', '2020-08-06');
Query OK, 1 row affected (0.00 sec)

mysql> SELECT * from student;
+------------+-------+--------+----------------+
| student_id | name  | gender | admission_date |
+------------+-------+--------+----------------+
|          1 | Jack  | M      | 2021-08-21     |
|          2 | Tom   | M      | 2020-08-21     |
|          3 | Cindy | F      | 2019-08-21     |
|       NULL | Ken   | NULL   | 2020-08-06     |
+------------+-------+--------+----------------+
4 rows in set (0.00 sec)
```







+ 如何設定為NOT NULL，也就是一定要填入數據

其實就是在我們創建table的時候，在我們要設定的column名稱後面加上NOT NULL

ex. 我們重新創建一個table，有name、gender、phone_number列，並限制name不能為空(NOT NULL)

```sql
mysql> create table student2(name VARCHAR(20) NOT NULL, gender ENUM("M","F"), phone_number VARCHAR(20));
Query OK, 0 rows affected (0.03 sec)
```

來看一下，這樣會有什麼改變

```sql
mysql> show columns from student2;
+--------------+---------------+------+-----+---------+-------+
| Field        | Type          | Null | Key | Default | Extra |
+--------------+---------------+------+-----+---------+-------+
| name         | varchar(20)   | NO   |     | NULL    |       |
| gender       | enum('M','F') | YES  |     | NULL    |       |
| phone_number | varchar(20)   | YES  |     | NULL    |       |
+--------------+---------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```

結果:可以看到我們name的NULL那行顯示為NO了，這樣就限制完成了

接著我們來試試，如果不輸入name的資訊會如何

```sql
mysql> INSERT into student2(gender) values ("M");
ERROR 1364 (HY000): Field 'name' doesn't have a default value
```

結果:它會顯示錯誤喔！！

透過這個NULL / NOT NULL的設定，我們就能限制說使用者在填入資料到我們的資料表時，哪些資料必填，哪些資料可以不用填喔







