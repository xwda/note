

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

找到之前订阅数据流出现错误的一个原因：发送方发送了加密数据，并且发送一个key，这个key每12个小时更新一次，然后某些时间这个key不可用，导致解析失败，后来数据流的下发者将数据放到明文字段了，但是没有通知数据订阅者，导致每天都会出现数据解析失败。

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

###### 02.19

event bus 用装饰器模式实现的。

###### 02.20

sync.Cond：一个协程在异步的接受数据，多个协程等这个协程接受完数据之后才能正确的读取数据。这种情况下用sync.cond实现比较好。

sync.Once：是go标准库提供的用于函数只调用一次的实现，常用与单例模式，例如初始化配置，保持数据库连接等，作用与init函数相似，但有区别。

​	init函数在程序在被首次加载时执行，若迟迟未被使用，不仅浪费了内存，又延长了程序的加载时间。

​	sync.Once可以在代码的任何位置初始化和调用，因此可以延时到使用时再执行，并发场景下是线程安全的。

sync.Pool：保存和复用临时对象，减少内存分配，减少GC压力。

控制协程并发数量：用sync.WaitGroup和channel来实现，另外一种是Jeffail/tunny和panjf2000/ants库来实现。

###### 02.21

传值会拷贝整个对象，而传指针只会拷贝指针，指向的是同一个对象。

传指针可以减少内存的拷贝，但会使得内存逃逸到堆上，给gc造成压力。在对象频繁创建和删除的场景中，传递指针带来的内存开销可能严重影响性能。

一般，对于需要修改的大对象，选择传指针。对于内存占用小结构体且只需要读的结构体选择传值。

###### 02.22

了解了go的性能测试工具bench和性能分析工具pprof。（pprof可以直观的查看性能图，和每行代码的耗时统计。pkg/profile封装了pprof的接口，易用性更强。）

###### 02.26

1. 看流式作业的项目代码
2. 看极客兔兔的geeRPC代码
3. 了解linux diff 命令的实现（目前知道这个命令有两种实现方式，实现的比较好的那种空间复杂度是O(ND)，时间复杂度是(ND)）
4. 想了一下目前词表的导出优化方案：
   1. 使用sync.Pool来优化内存，减轻gc压力，从而提高batchsize和并发量，从而减少导出时间。
   2. diff文件的中的一个在afs上，一个在本机上，将afs上的拷贝一份放在本地，然后diff可能会快点。
   3. diff和scp的时候用并发来提升效率。
   4. 看一下linux diff命令的实现方式，看能不能在特定的场景下优化。



