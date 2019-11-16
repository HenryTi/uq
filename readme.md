# UQ语法

## 数据类型
uq支持以下数据类型
### 数字类型
#### tinyint
1字节8位的整型数据。范围是-128到127。
#### smallint
2字节16位的整型数据。范围是-32768到32767。
#### int
4字节32位的整型数据。范围是-2147483648到2147483647。
#### bigint
8字节64位的整型数据。带符号的范围是-9223372036854775808到9223372036854775807。
#### dec
`dec(p,d)`表示精度p，小数位d
P是表示有效数字数的精度。 P范围为1〜65。
D是表示小数点后的位数。 D的范围是0~30。MySQL要求D小于或等于(<=)P。

### 文字类型
#### char
CHAR(M) 是长度可变的字符串，M 表示最大列的长度，M 的范围是 0～65535。
#### text
TEXT 表示长度为 65535（216-1）字符的 TEXT 列。

### 日期时间
#### datetime
#### date
#### time

### stamp
#### stamp

## TUID：定义基础信息
### 代码样例
    TUID 商品 GLOBAL (
        id,
        main name char(50),
        d_number dec,
        discription text,
        unique (name),
        search (name, discription),
    );
### 说明
+   TUID 定义基础信息，会在数据库中产生唯一id。
+   第一个字段定义是id，不需要定义名字，也不需要定义类型。默认数据类型是bigint。
+   main类型字段。所有的main字段类型，在基本的显示时，uq客户端会自动获取展示。
+   uqinue唯一性定义。比如名字或者编号。
+   search搜索字段。搜索字段在search时，会自动对应like搜索。
+   global，全局tuid，公开的，而且是唯一的

## MAP：对照表
### 代码样例
    MAP CustomerArticle ver 0.4 (
        key customer id Customer,
        key article id Article,
        num dec(12,3),
    );
### 说明



