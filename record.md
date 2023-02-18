

[toc]



###### 02.09

mysql中insert，insert ignore, replace的区别

###### 02.10

golang 中反射：

三法则：从interface{}可以反射出反射对象；从反射对象中可以获取interface{}；要修改反射对象，其原值必须可修改。

###### 02.11

geerpc 超时处理 

###### 02.12

工作

###### 02.13

geerpc，内存对齐

###### 02.14

golang中byte，rune，string的区别

byte是uint8的别称，占一个字节的存储空间。byte是为了将byte的值和uint8的整数区分开来。

rune是int32的别称，占四个字节的存储空间。rune是为了将字符值和int32的整数区分开来。rune中存储的是字符在unicode编码的字符。

string是byte的集合，字符串可以是"",但不能是nil。string的值是不能被改变，string的拼接会指向一个新的内存空间。string的对象赋值不占用额外的内存空间。只是将指针指向同一个stringHeader。

###### 02.16

mysql  join

```mysql
SELECT * FROM t1 LEFT JOIN (t2, t3, t4) ON (t2.a = t1.a AND t3.b = t1.b AND t4.c = t1.c)

SELECT * FROM table1, table2;

SELECT * FROM table1 INNER JOIN table2 ON table1.id = table2.id;

SELECT * FROM table1 LEFT JOIN table2 ON table1.id = table2.id;

SELECT * FROM table1 LEFT JOIN table2 USING (id);

SELECT * FROM table1 LEFT JOIN table2 ON table1.id = table2.id LEFT JOIN table3 ON table2.id = table3.id;
```

group by 的用法：

```mysql
SELECT a, b, COUNT(c) AS t FROM test_table GROUP BY a,b ORDER BY a,t DESC;
```

insert select联合使用的方法：

```mysql
INSERT INTO tbl_temp2 (fld_id)
  SELECT tbl_temp1.fld_order_id
  FROM tbl_temp1 WHERE tbl_temp1.fld_order_id > 100;
```

###### 02.17

调研watt和DTS的服务。

找到之前订阅数据流出现错误的一个原因：发送方发送了加密数据，并且发送一个key，这个key没12个小时更新一次，然后某些时间这个key不可用，导致解析失败，后来数据流的下发者将数据放到明文字段了，但是没有通知数据订阅者，导致每天都会出现数据解析失败。

mysql和table的区别：

- mysql是关系型数据库。

- table是非关系数据库，然后table可以存很多行。

###### 02.18

mysql kill 命令

```mysql
KILL [CONNECTION | QUERY] *processlist_id*
```

kill connect 和 kill query 的区别：

- kill connect 与没有任何修饰的kill一样，先断开这个线程id的连接，然后终止这个线程执行的语句。
- kill query 终止连接正在执行的语句，但是保持连接的完整性。