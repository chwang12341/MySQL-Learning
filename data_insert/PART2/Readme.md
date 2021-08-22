# MySQL 學習筆記(六) - 如何限制Table中哪些欄位在插入數據時必須是唯一值? 當沒有插入任何數據時，怎麼設定預設值? 如何限制哪些列的組合是唯一組合就好? Default, Primary Key, UNIQUE 用法





## 前情提要

在上一篇中，我們創立了一個database叫demo，並於demo創立了兩個table為student和student2，但大家放心我們可以不用到之前創建的table，但是這邊我就不對怎麼創建database，怎麼創建table多做解釋了喔!!

```sql
mysql> show tables;
+----------------+
| Tables_in_demo |
+----------------+
| student        |
| student2       |
+----------------+
2 rows in set (0.00 sec)
```



## 1. Default 用法 - 如何設定預設值?

前面一篇，我們帶大家了解如何將某些column設置為必填(NOT NULL)，這邊我們希望的狀況是可以為NULL，也就是使用者可以在這列中不填入任何資訊，但是我們希望系統可以自動給予一個預設值，也就是當使用者不填入此資訊時，我們會自動補一個我們設定好的預設值當此欄位資訊

+ 在我們創建TABLE時，在我們要設定的column後面寫上一個DEFAULT `預測值`

這邊我們假設將phone的預設值設為"123456"

```sql
create table student3(name VARCHAR(20), phone VARCHAR(20) DEFAULT "123456", admission_data DATE);
```

顯示student3資料表資訊

```sql
mysql> desc student3;
+----------------+-------------+------+-----+---------+-------+
| Field          | Type        | Null | Key | Default | Extra |
+----------------+-------------+------+-----+---------+-------+
| name           | varchar(20) | YES  |     | NULL    |       |
| phone          | varchar(20) | YES  |     | 123456  |       |
| admission_data | date        | YES  |     | NULL    |       |
+----------------+-------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```

我們插入一組數據，但phone不設定試試

```sql
mysql> insert into student3(name, admission_data) values ("Jack", "2021-09-08");
Query OK, 1 row affected (0.01 sec)
```

查看資料表中的數據

```sql
mysql> SELECT * FROM student3;
+------+--------+----------------+
| name | phone  | admission_data |
+------+--------+----------------+
| Jack | 123456 | 2021-09-08     |
+------+--------+----------------+
1 row in set (0.00 sec)
```

結果: 可以看出我們的phone數據被自動補上"123456"了!!



## 2.  Primary Key 用法 - 設定唯一值 - 如何強迫使用者在填入某個欄位的數據時，不可以跟資料表中有的重複?

可能情境: 有些欄位不可能會有一樣的數據，像是我們如果有一個欄位是身分證字號，我們插入的數據中，不可能會有兩個不同的人，有一樣的身分證字號，這時候我們就需要將身分證這個column設置為PRIMARY KEY，這樣當有重複狀況出現的時候，就會不讓使用者插入這筆重複數據



**解法一: 在創立table的時候，我們在後面加上PRIMARY KEY(`要設定為Primary Key的column name`)**

這邊我們創建一個table並把學生ID設定為Primary Key，因為學生id理論來說不應該有兩個一樣的情況

```sql
mysql> create table student4(name VARCHAR(20), phone VARCHAR(20), student_id INT, PRIMARY KEY (student_id));
Query OK, 0 rows affected (0.04 sec)
```





**解法二: 在創立table的時候，在我們不希望有重複情況出現的欄位後面加上PRIMARY KEY**

```SQL
mysql> create table student5(name VARCHAR(20), phone VARCHAR(20), student_id INT PRIMARY KEY);
Query OK, 0 rows affected (0.04 sec)
```







**結果: 這時候我們查看一下student4的資料表資訊，就會看到student_id中的Key被設定為PRI，而且NULL那欄會被設定為NO，也就是用Primary Key設定為唯一值的欄位，必須填入數據**

```sql
mysql> desc student4;
+------------+-------------+------+-----+---------+-------+
| Field      | Type        | Null | Key | Default | Extra |
+------------+-------------+------+-----+---------+-------+
| name       | varchar(20) | YES  |     | NULL    |       |
| phone      | varchar(20) | YES  |     | NULL    |       |
| student_id | int(11)     | NO   | PRI | NULL    |       |
+------------+-------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```





接下來我們來插入數據試試，首先我們先插入一筆數據，接著再插入一筆與前一筆的student_id一樣的數據，看看會發生什麼事

```sql
mysql> insert into student4(name, phone, student_id) values ("Tom", "123456", 1);
Query OK, 1 row affected (0.01 sec)

mysql> insert into student4(name, phone, student_id) values ("Jessica", "1232356", 1);
ERROR 1062 (23000): Duplicate entry '1' for key 'PRIMARY'
```



結果: 第二筆數據插入的時候，發生錯誤了，因為student_id要為唯一值，不可以出現重複的



## 3. AUTO_INCREMENT 用法 - 如何自動生成一組數據?

在我們要自動生成數據的column後面加上 AUTO_INCREMENT

+ 假設我們今天想讓student_id變成自動生成，並且把它設定為唯一值

```sql
mysql> create table student6(name VARCHAR(20), phone VARCHAR(20), student_id INT AUTO_INCREMENT, PRIMARY KEY (student_id));
Query OK, 0 rows affected (0.04 sec)
```

+ 設置好後，我們隨便插入兩組數據，然後不設定student_id

```sql
mysql> desc student6;
+------------+-------------+------+-----+---------+----------------+
| Field      | Type        | Null | Key | Default | Extra          |
+------------+-------------+------+-----+---------+----------------+
| name       | varchar(20) | YES  |     | NULL    |                |
| phone      | varchar(20) | YES  |     | NULL    |                |
| student_id | int(11)     | NO   | PRI | NULL    | auto_increment |
+------------+-------------+------+-----+---------+----------------+
3 rows in set (0.00 sec)

mysql> insert into student6(name, phone) values ("Jessica", "1232356");
Query OK, 1 row affected (0.00 sec)

mysql> insert into student6(name, phone) values ("Jenny", "1241313432356");
Query OK, 1 row affected (0.00 sec)

mysql> SELECT * FROM student6;
+---------+---------------+------------+
| name    | phone         | student_id |
+---------+---------------+------------+
| Jessica | 1232356       |          1 |
| Jenny   | 1241313432356 |          2 |
+---------+---------------+------------+
2 rows in set (0.00 sec)
```

結果: 就會看到student_id被自動填入數字了，神奇吧!!



## 4. UNIQUE 用法 - 設定唯一值，但是使用者可以不插入值

可能情境: Primary Key雖然可以幫我們設定唯一值，但是它必須填入數據，沒辦法不填入，這時候我們如果想要有這種功能，就可以使用UNIQUE



+　在創建Table的時候，在要設定為唯一值的欄位後面加上UNIQUE

我們創建一個資料表name設定為Primary Key, phone設定為UNIQUE

```SQL
mysql> create table student7(name VARCHAR(20) PRIMARY KEY, phone VARCHAR(20) UNIQUE);
Query OK, 0 rows affected (0.07 sec)
```

+　這時候不填入name的數據，只填入phone的數據

```sql
mysql> insert into student7(phone) values ("1241313432356");
ERROR 1364 (HY000): Field 'name' doesn't have a default value
```



結果: 會報錯，因為PRIMARY KEY一定要填入數據



+　接著填入name的數據，不填入phone的數據

```sql
mysql> insert into student7(name) values ("Mandy");
Query OK, 1 row affected (0.00 sec)
```



結果: 不會報錯，因為UNIQUE允許不填入數據

```sql
mysql> SELECT * FROM student7;
+-------+-------+
| name  | phone |
+-------+-------+
| Mandy | NULL  |
+-------+-------+
1 row in set (0.00 sec)
```





## 5. Primary Key 不同用法 - 怎麼設定來讓多個欄位的組合是唯一值就好?

**可能情境:**有時候我們能接受某個欄位有重複的數據插入，但是我們不希望有兩個欄位的組合重複插入，像是我們覺得姓名可以重複，地址也可以重複，但是同樣姓名又同樣地址這樣就不行，這時候我們就需要用到這個方法





**可能問題**

上面我們教大家使用兩種方法來設定Primary Key, 但是在這種情況下我們只能使用在最後面加上**PRIMARY KEY(`要設定為Primary Key的column name`)**的方法，因為如果我們使用第二種方法會報錯



這邊我們想將name跟address設定為一個唯一組合

+　使用第二種方法，來設定兩個Priamry Key

```sql
mysql> create table student8(name VARCHAR(20) PRIMARY KEY, address VARCHAR(20) PRIMARY KEY);
ERROR 1068 (42000): Multiple primary key defined
```



結果: 會報錯喔

+　使用第一種方法，來設定兩個Priamry Key

```sql
mysql> create table student8(name VARCHAR(20), address VARCHAR(20), PRIMARY KEY(name, address));
Query OK, 0 rows affected (0.04 sec)

mysql> desc student8;
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| name    | varchar(20) | NO   | PRI | NULL    |       |
| address | varchar(20) | NO   | PRI | NULL    |       |
+---------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```



結果: 設定成功



+ 插入兩筆只有name一樣的數據

```sql
mysql> insert into student8(name, address) values ("Mandy", "street");
Query OK, 1 row affected (0.00 sec)

mysql> insert into student8(name, address) values ("Mandy", "street1");
Query OK, 1 row affected (0.00 sec)
```





+ 插入兩筆只有address一樣的數據

```sql
mysql> insert into student8(name, address) values ("Ken", "street");
Query OK, 1 row affected (0.00 sec)

mysql> insert into student8(name, address) values ("Tom", "street");
Query OK, 1 row affected (0.00 sec)
```





結果: 上面的插入都不會報錯喔，因為我們允許單一一樣，但不允許組合一樣

+　插入兩筆name跟address一樣的數據

```sql
mysql> insert into student8(name, address) values ("Amy", "street-1");
Query OK, 1 row affected (0.00 sec)

mysql> insert into student8(name, address) values ("Amy", "street-1");
ERROR 1062 (23000): Duplicate entry 'Amy-street-1' for key 'PRIMARY'
```





結果: 會報錯喔，因為組合一樣了

提醒: 組合不一定只限制兩個喔，大家可以根據自己需求設定唯一組合有幾個column



