# SQL操作(mac)

## 连接数据库

1. 开启数据库

    系统偏好设置 > MySQL > start MySQL server

2. 终端打开SQL

    输入命令 /usr/local/MySQL/bin/mysql -u 数据库用户名 -p；

    按照提示输入密码

## SQL语句

1. 数据库的操作

    1、退出数据库命令： exit

    2、查询所有的数据库： show databases; (注意：这里一定要加s和；)

    3、创建数据库 create database 数据库的名称 [ character set 编码]（可有可无，如果没有的话，就是这个服务器的默认编码）

        没有编码格式的：create database abintest;
        加编码的： create database my_datatest character set gbk; (创建gbk编码格式的数据库)。

    4、查看当前数据库的编码： show create database 数据库名称；

    5、修改数据库的编码：alter database 数据库名称 character set 编码形式；

    6、删除数据库：drop database 数据库名称；

    7、修改数据库名称： rename database 旧的名称 to 新的数据库名称（好像不管用）还可以：进入mysql的文件夹，然后改一下数据库的文件夹名称（待测试）

    8、切换数据库： use 数据库名称；

    9、查看当前使用的数据库： select database();

2. 表的操作

    1、创建数据表：

        create table 数据表名称 （字段1 数据类型，字段2，字段3...）；

        数据类型有：int、varchar、double、bit、datatime

    2、查看数据表：

        show tables；

    3、查看数据表编码：
        show create table 数据表名；

    4、修改数据表编码：

        alter table 数据表名称 character set 编码；

    5、查看数据表结构：

        desc 数据表名称；

    6、修改数据表结构

        （1）增加列，添加一个字段
            alter table 数据表名称 add 字段名称 字段类型；
        
        （2）修改长度/类型/约束
            alter table 数据表的名称 modify 字段名称 新的类型（新的长度）;

        （3）修改列名（修改字段名称）
            alter table 数据表名称 change 旧字段名 新的字段名 类型（长度）；

        （4）删除列（删除字段）
            alter table 数据表名称 drop 字段名称；

    7.修改数据表名称：

        rename table 旧的数据表名称 to 新数据表名称；

    8.主键约束 主键的约束就是为了保证一个列数据不重复。 用primary key修饰。一般来说，一个表只有一个主键

        （1）添加主键（在现有字段上添加主键）：alter table 数据表名称 modify 字段名称 类型 primary key；
        （2）新建字段的时候添加主键：alter table 数据表名称 add 字段名 字段类型 primary key；
            例如：alter table person add id int primary key；
        （3） 可以让主键自动增加auto_increment；
            例如：alter table person add id int primary key auto_increment；

    9.唯一约束，是为了保证数据不重复，与主键不同的是可以控制多个字段不重复。

        例如：create table student(id int primary key auto_increment,name varchar(20),age int,stu_number varchar(20) unique, sex varchar(10));

    10.非空约束 ，就是被约束的字段必须有数据，不能为空。

        （1）创建数据表时，可以直接在字段类型后添加not null。
            例如： create table student(id int primary key auto_increment,name varchar(20),age int not null,stu_number varchar(20) unique, sex varchar(10) not null);
        （2）修改字段不能为空
            alter table 数据表 modify 字段 字段名 not null；

    11.删除主键约束/唯一约束/非空约束

        （1）删除主键(注意：如果主键是自动增长的，会报错。需要先把自动增长删除)
            alter table 表名 drop primary key；
            需要先修改自动增长
            alter table student modify id int；
        （2）删除唯一约束
            alter table 表名 drop index 字段名称；
        （3）删除非空约束， 直接删除即可
            alter table 表名 modify 字段 字段类型；

    12.删除表

        drop table 表名；

3. 数据的操作

    1.插入完整数据（增）（注意主键是自动增长的，第二次输入数据时可以不用设置值）

        （1）insert into 表名（字段名称1，字段名称2...）values(值1，值2...)；

        （2）如果插入全部数据，则可以省略前面的字段名称，

            insert into 表名 values （值1，值2...）；

        （3）插入部分数据，直接输入字段和对应的值就可以了

            insert into person （name，age） values （“哈哈”,20）;

    2.查询表中的数据（查）

        （1）select * from 表名；

        （2）select * from 表名 [where 条件]；

            条件：=、<>、>=、<=、>、<、 is null、is not null、 and、or、like

        （3）筛选出需要的列表
        
            select 列名，列名 from 表名 [where 条件];

        （4）排序有两种：升序和降序；

            select 列名，列名... from 表名 order by 列名 asc(升序)| desc (降序)；

        （5）别名，好处是可以根据我们自己的需要来显示字段
        
            select 列名 as 别名，列名 as 别名 from 表名；

        （6）模糊查询

            like 通配符

            _ 占位符 （一个代表一个字符长度）

    3.修改数据（改）对数据进行更新

        (1) update 表名 set 字段=值 条件（where）；

        (2) 当判断为空的时候，不能使用=null，应该用 is null；

    4.删除表中的数据（删）

        delete from 表名 条件

    5.函数

        导入数据库：
        1.打开终端输入 mysql -u root -p

        2.show databases;

        3.use 数据库名;

        4.source 将sql文件拖入终端; （不要忘记后边的分号）

        5.回车，即可导入成功