# MySQL 學習筆記(三) - 理解Database的組成 - Database與Table之間的關聯 - MySQL Database的基本指令教學



## 1. Database的組成? Database 與 Table 間的關聯?

+ Database是由一堆的Tables(一到多個)所組成(通常tables之間有關係，才會被聚集在一起，而MySQL可以創建很多個database)
+ Tables用來存儲資料(data)，而在MySQL中Table是有結構的，指的是在Table中插入的數據需要有一定的結構化標準，而結構就是指的Table的結構



## 2. Database Level的基本指令



| 指令                      | 說明                                            |
| ------------------------- | ----------------------------------------------- |
| show databases;           | 顯示所有資料庫                                  |
| create database `<name>`; | 創建資料庫，並指定其名稱(**提醒:名稱不能空格**) |
| drop database `<name>`;   | 刪除指定名稱的資料庫                            |
| use `<database name>`;    | 指定使用哪個資料庫(切換資料庫)                  |
| select database();        | 查詢當前使用的資料庫是哪一個                    |

+ 筆記: 指令後面要加一個`;`



### show databases;

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| test               |
| world              |
+--------------------+
7 rows in set (0.00 sec)
```

裡面為目前有的資料庫(ex. information_schema、mysql、performance_schema、sakila、sys、test、world)



### create database `<name>`;

+ 創建一個名稱為my_db1

```
mysql> create database my_db1;
Query OK, 1 row affected (0.00 sec)
```

檢視當前所有數據庫

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| my_db1             |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| test               |
| world              |
+--------------------+
8 rows in set (0.00 sec)
```

我們剛剛創建的my_db1就顯示在裡面了



+ 錯誤示範: 名稱使用空格

```
mysql> create database my db1;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'db1' at line 1
```

這樣就會報錯喔



### drop database `<name>`;

+ 刪掉剛剛創建的my_db1資料庫

```
mysql> drop database my_db1;
Query OK, 0 rows affected (0.00 sec)
```

檢視當前所有數據庫

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| test               |
| world              |
+--------------------+
7 rows in set (0.00 sec)
```

my_db1就被拿掉了喔!!



### use `<database name>`;

+ 我們先再創建一個database才能使用(切換)它

```
mysql> create database my_db1;
Query OK, 1 row affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| my_db1             |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| test               |
| world              |
+--------------------+
8 rows in set (0.00 sec)
```



+ 切換至my_db1資料庫

```
mysql> use my_db1;
Database changed
```



### select database();

+ 查看當前使用的資料庫

```
mysql> select database();
+------------+
| database() |
+------------+
| my_db1     |
+------------+
1 row in set (0.00 sec)
```



+ 當我們還沒選擇切換至任何資料庫，它就會顯示為Null

```
mysql> drop database my_db1;
Query OK, 0 rows affected (0.00 sec)

mysql> select database();
+------------+
| database() |
+------------+
| NULL       |
+------------+
1 row in set (0.00 sec)
```



## 3. 怎麼改變分隔符delimiter(;)? 為什麼需要分隔符(;)?

**為什麼需要分隔符(;)?**

+ 與許多程式語言一樣，我們都需要告訴系統我們已經寫完命令，如何告知?就是透過分隔符delimiter，而MySQL的預設分隔符為`;`，這也是為什麼在寫任何指令，執行的時候都需要在後面加上`;`，如果不寫`;`那系統就會以為你還沒寫完指令，因此就還不會執行它



沒有打上`;`就按Enter執行的狀況

```
mysql> show databases
    ->
    ->
```



系統就會以為你還要繼續寫

加上`;`分隔符，告訴系統你已經寫好指令

```
mysql> show databases
    ->
    -> ;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| test               |
| world              |
+--------------------+
7 rows in set (0.00 sec)
```





**怎麼改變分隔符delimiter(;)?**

+ 使用`delimiter <要設定成分隔符的符號>`
+ 例子: 我們將`&&`設定成分隔符

```
mysql> delimiter &&
mysql> show databases&&
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| test               |
| world              |
+--------------------+
7 rows in set (0.00 sec)
```





## 4. SQL語句寫大小寫會影響嗎?

大寫與小寫在SQL中是都一樣的喔

```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| test               |
| world              |
+--------------------+
7 rows in set (0.00 sec)

mysql> SHOW databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| test               |
| world              |
+--------------------+
7 rows in set (0.00 sec)

mysql> show DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| test               |
| world              |
+--------------------+
7 rows in set (0.00 sec)
```

關於大小寫應該要怎麼使用比較好，大家可以參考下面網路上非常厲害人的寫法，他將依些關鍵字用大寫來書寫，會讓整個語句看起來比較明顯清楚

![image1](images1\image1.PNG)



參考網址: https://stackoverflow.com/questions/292026/is-there-a-good-reason-to-use-upper-case-for-sql-keywords



