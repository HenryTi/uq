### UQ语法

#### TUID
##### 代码样例
    TUID 商品 GLOBAL (
        id,
        base b1 id article,
        main name char(50),
        d2 dec,
        discription text,
        unique (name),
        search (name, discription),
    );
##### 说明
+   TUID 定义基础信息，会在数据库中产生唯一id。
+   第一个字段定义是id，不需要定义名字，也不需要定义类型。类型是bigint。

