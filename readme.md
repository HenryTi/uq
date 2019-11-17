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
### 用途
+   定义基础信息，会在数据库中产生唯一id。
+   可以包含子级定义。形成基础数据的主子关系。
### 代码样例
    TUID 商品 GLOBAL (
        id,
        MAIN name char(50),
        d_number dec,
        discription text,
        ARR pack (
            owner,
            id,
            cat char(50) not null,
            MAIN radiox dec(12,2),
            MAIN radioy dec(12,4),
            MAIN unit char(10),
        ),
        UNIQUE (name),
        SEARCH (name, discription),
    );
### 说明
+   第一个字段定义是id，不需要定义名字，也不需要定义类型。默认数据类型是bigint。
+   MAIN类型字段。所有的main字段类型，在基本的显示时，uq客户端会自动获取展示。
+   ARR是Tuid下的子级id。arr等同于一个tuid，但是这个tuid的存在必须依赖owner的存在。
+   UNIQUE唯一性定义。比如名字或者编号。
+   SEARCH搜索字段。搜索字段在search时，会自动对应like搜索。
+   GLOBAL，全局tuid，公开的，而且是唯一的

## MAP：对照表
### 用途
+   赋予TUID属性。
+   构造多个TUID之间的关系。
### 代码样例
    MAP CustomerArticle ver 0.4 (
        KEY customer ID Customer,
        KEY article ID Article,
        num dec(12,3),
    );
### 说明
+   必须包含一个或者多个key字段。key字段通常情况下应该是id字段。
+   可以有其它字段，也可以没有。

## SHEET：单据

## ACTION：操作
### 用途
### 代码样例
    ACTION TestAction (
        b ID t,
        c ID t1,
        ARR arr1 (
            t1 int,
            k char(50),
        ),
    )
    RETURNS a1 (t int, k char(50))
    {
        VAR s int = 2, s2 dec = 3;
        SET s = 1;
        FOREACH (var v1 int, v2 dec of select 1, 2) {
            SET s = v1;
            SET s2 = v2;
        }
    };

### 说明
+   ARR 表参量
+   RETURNS 返回表，包含表的字段

## QUERY：数据查询
### 代码样例
    QUERY 查询Test (
        id1 id article,
        text1 char(80)
    )
    RETURNS ret1 (a int, b text)
    PAGE (date datetime start now(), p1 int, p2 char(50))
    {
        -- book 账 at (id1, id1) set f1+=b1, f2=b1; -- where 1<>1;
        into ret1 select a.e1 as a, 1 as b from 流水 as a;
        page select a.[date], a.e1 as p1, 3 as p2 
            from 流水 as a 
            where a.k1=5 and a.[date]<$pageStart
            order by a.date desc
            limit $pageSize;
    };
### 说明
+   RETURNS 返回表，包含表的字段
+   PAGE 分页返回表，start 分页的起点
+   page select limit $pageSize，表的行限制

## PENDING：待处理
### 说明
pending是一种过渡帐。待处理的数据，可以存放在这个帐上。这个帐上的数据，可以一步步操作修改。操作完成就可以删除。

### 代码样例
    PENDING Receivable ver 0.6 (
        id,
        customer ID Customer,
        product ID Product,
        pack OF product.Pack,
        price dec(12,2),
        quantity int,
        amount dec(12,2),
        user,
        date,
        sheet,
        type,
        row,
    );
### 操作中的语法
#### 加操作
    PENDING Receivable +(customer:1, product:2, pack:2, price:3.5*bbb) to recId;

#### 删除操作
    PENDING Receivable -at recId;

#### 减操作
    PENDING Receivable -(price:2.5+amount) at recId del if 3*2=amount;

### 说明
+   id，待处理项的id
+   没有key，没有索引
+   user，在sheet action中写pending时，操作的user
+   date，在sheet action中写pending时，操作的date
+   sheet，在sheet action中写pending时，操作的sheet的id
+   type，在sheet action中写pending时，操作的sheet的type
+   row，在sheet action中写pending时，操作的sheet的行号

## BOOK：数据帐

## HISTORY：流水账

## 过程中的语句
+   VAR 定义变量
+   SET 设置
+   SELECT
+   FOREACH 循环
+   IF ELSE
+   LEAVE
+   CONTINUE
+   BOOK
+   PENDING
+   HISTORY
+   BUS
+   SEND

## Markdown 表格样例

### 以|描述的表格

| 一个普通标题 | 一个普通标题 | 一个普通标题 |
| ------ | ------ | ------ |
| 短文本 | 中等文本 | 稍微长一点的文本 |
| 稍微长一点的文本 | 短文本 | 中等文本 |

### 以`<table><tr><td>`描述的表格，撑满页

<table style="width:100%;">
<tr>
<td>文本</td>
<td>顶行</td>
<td>dddd</td>
<td>dddd</td>
</tr>
<tr>
<td>dddd</td>
<td>另外一列文本</td>
<td>dddd</td>
<td>文本列</td>
</tr>
<tr>
<td style="text-align:right;">底列靠右</td>
<td>列文本</td>
<td>dddd</td>
<td>dddd</td>
</tr>
</table>

