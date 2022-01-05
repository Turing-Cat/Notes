# mysql

## day01课堂笔记

### 1、数据库概述

什么是数据库？什么是数据库管理系统？什么是SQL？他们之间的关系是什么？

	数据库：
		英文单词DataBase，简称DB。按照一定格式存储数据的一些文件的组合。
		顾名思义：存储数据的仓库，实际上就是一堆文件。这些文件中存储了
		具有特定格式的数据。
	
	数据库管理系统：
		DataBaseManagement，简称DBMS。
		数据库管理系统是专门用来管理数据库中数据的，数据库管理系统可以
		对数据库当中的数据进行增删改查。
	
		常见的数据库管理系统：
			MySQL、Oracle、MS SqlServer、DB2、sybase等....
	
	SQL：结构化查询语言
		程序员需要学习SQL语句，程序员通过编写SQL语句，然后DBMS负责执行SQL
		语句，最终来完成数据库中数据的增删改查操作。
	
		SQL是一套标准，程序员主要学习的就是SQL语句，这个SQL在mysql中可以使用，
		同时在Oracle中也可以使用，在DB2中也可以使用。
	
	三者之间的关系？
		DBMS--执行--> SQL --操作--> DB
	
	先安装数据库管理系统MySQL，然后学习SQL语句怎么写，编写SQL语句之后，DBMS
	对SQL语句进行执行，最终来完成数据库的数据管理。



### 2、安装MySQL数据库管理系统。

​	第一步：先安装，选择“经典版”
​	第二步：需要进行MySQL数据库实例配置。

	注意：一路下一步就行了！！！！！
	
	需要注意的事项？
		端口号：
			端口号port是任何一个软件/应用都会有的，端口号是应用的唯一代表。
			端口号通常和IP地址在一块，IP地址用来定位计算机的，端口号port
			是用来定位计算机上某个服务的/某个应用的！
			在同一台计算机上，端口号不能重复。具有唯一性。
	
			mysql数据库启动的时候，这个服务占有的默认端口号是3306
			这是大家都知道的事儿。记住。
		
		字符编码方式？
			设置mysql数据库的字符编码方式为 UTF8
			一定要注意：先选中第3个单选按钮，然后再选择utf8字符集。
		
		服务名称？
			默认是：MySQL
			不用改。
		
		选择配置环境变量path：
			如果没有选择怎么办？你可以手动配置
			path=其它路径;C:\Program Files (x86)\MySQL\MySQL Server 5.5\bin
		
		mysql超级管理员用户名不能改，一定是：root
		你需要设置mysql数据库超级管理员的密码。
		我们设置为123456
	
		设置密码的同时，可以激活root账户远程访问。
		激活：表示root账号可以在外地登录。
		不激活：表示root账号只能在本机上使用。
		我这里选择激活了！



### 3、MySQL数据库的完美卸载！

​	第一步：双击安装包进行卸载删除。
​	第二步：删除目录：
​		把C:\ProgramData下面的MySQL目录干掉。
​		把C:\Program Files (x86)下面的MySQL目录干掉。

	这样就卸载结束了！





### 4、设置mysql服务

看一下计算机上的服务，找一找MySQL的服务在哪里？
	计算机-->右键-->管理-->服务和应用程序-->服务-->找mysql服务
	MySQL的服务，默认是“启动”的状态，只有启动了mysql才能用。
	默认情况下是“自动”启动，自动启动表示下一次重启操作系统的时候
	自动启动该服务。

	可以在服务上点击右键：
		启动
		重启服务
		停止服务
		...
	
	还可以改变服务的默认配置：
		服务上点击右键，属性，然后可以选择启动方式：
			自动（延迟启动）
			自动
			手动
			禁用



### 5、开启关闭服务

在windows操作系统当中，怎么使用命令来启动和关闭mysql服务呢？
	语法：
		net stop 服务名称;
		net start 服务名称;

	其它服务的启停都可以采用以上的命令。

### 6、连接mysql

mysql安装了，服务启动了，怎么使用客户端登录mysql数据库呢？
	使用bin目录下的mysql.exe命令来连接mysql数据库服务器

	本地登录（显示编写密码的形式）：
		C:\Users\Administrator>mysql -uroot -p123456
		mysql>
	
	本地登录（隐藏密码的形式）：
		C:\Users\Administrator>mysql -uroot -p
		Enter password: ******
	
		mysql>

### 7、mysql常用命令：

	退出mysql ：exit
	
	查看mysql中有哪些数据库？
		show databases; 
		注意：以分号结尾，分号是英文的分号。
	
	mysql> show databases;
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| mysql              |
	| performance_schema |
	| test               |
	+--------------------+
	mysql默认自带了4个数据库。
	
	怎么选择使用某个数据库呢？
		mysql> use test;
		Database changed
		表示正在使用一个名字叫做test的数据库。
	
	怎么创建数据库呢？
		mysql> create database bjpowernode;
		Query OK, 1 row affected (0.00 sec)
	
		mysql> show databases;
		+--------------------+
		| Database           |
		+--------------------+
		| information_schema |
		| bjpowernode        |
		| mysql              |
		| performance_schema |
		| test               |
		+--------------------+
	
	查看某个数据库下有哪些表？
		mysql> show tables;
	
	注意：以上的命令不区分大小写，都行。
	
	查看mysql数据库的版本号：
	mysql> select version();
		+-----------+
		| version() |
		+-----------+
		| 5.5.36    |
		+-----------+
	
	查看当前使用的是哪个数据库？
	mysql> select database();
	+-------------+
	| database()  |
	+-------------+
	| bjpowernode |
	+-------------+
	
	mysql> show
	-> databases
	-> ;
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| bjpowernode        |
	| mysql              |
	| performance_schema |
	| test               |
	+--------------------+
	
	注意：mysql是不见“;”不执行，“;”表示结束！
	
	mysql> show
	->
	->
	->
	->
	->
	->
	->
	->
	-> \c
	mysql>
	\c用来终止一条命令的输入。



8、**数据库当中最基本的单元是表：table**

	什么是表table？为什么用表来存储数据呢？
	
		姓名	性别	年龄(列：字段) 
		---------------------------
		张三	男			20            ------->行（记录）
		李四	女			21            ------->行（记录）
		王五	男			22            ------->行（记录）
	
	数据库当中是以表格的形式表示数据的。
	因为表比较直观。
	
	任何一张表都有行和列：
		行（row）：被称为数据/记录。
		列（column）：被称为字段。
	
	姓名字段、性别字段、年龄字段。
	
	了解一下：
		每一个字段都有：字段名、数据类型、约束等属性。
		字段名可以理解，是一个普通的名字，见名知意就行。
		数据类型：字符串，数字，日期等，后期讲。
	
		约束：约束也有很多，其中一个叫做唯一性约束，
			这种约束添加之后，该字段中的数据不能重复。

### 9、关于SQL语句的分类？

	SQL语句有很多，最好进行分门别类，这样更容易记忆。
		分为：
			DQL：
				数据查询语言（凡是带有select关键字的都是查询语句）
				select...
	
			DML：
				数据操作语言（凡是对表当中的数据进行增删改的都是DML）
				insert delete update
				insert 增
				delete 删
				update 改
	
				这个主要是操作表中的数据data。
	
			DDL：
				数据定义语言
				凡是带有create、drop、alter的都是DDL。
				DDL主要操作的是表的结构。不是表中的数据。
				create：新建，等同于增
				drop：删除
				alter：修改
				这个增删改和DML不同，这个主要是对表结构进行操作。
	
			TCL：
				不是王牌电视。
				是事务控制语言
				包括：
					事务提交：commit;
					事务回滚：rollback;
	
			DCL：
				是数据控制语言。
				例如：授权grant、撤销权限revoke....

### 10、导入数据

​	bjpowernode.sql 这个文件中是我提前为大家练习准备的数据库表。
​	怎么将sql文件中的数据导入呢？
​		mysql> source D:\course\03-MySQL\document\bjpowernode.sql

		注意：路径中不要有中文！！！！

### 11、关于导入的这几张表？

​	mysql> show tables;
​	+-----------------------+
​	| Tables_in_bjpowernode |
​	+-----------------------+
​	| dept                  |
​	| emp                   |
​	| salgrade              |
​	+-----------------------+

	dept是部门表
	emp是员工表
	salgrade 是工资等级表
	
	怎么查看表中的数据呢？
		select * from 表名; //统一执行这个SQL语句。
	
	mysql> select * from emp; // 从emp表查询所有数据。
	+-------+--------+-----------+------+------------+---------+---------+--------+
	| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
	+-------+--------+-----------+------+------------+---------+---------+--------+
	|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
	|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
	|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
	|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
	|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
	|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
	|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
	|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
	|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
	|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
	|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
	|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
	|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
	|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
	+-------+--------+-----------+------+------------+---------+---------+--------+
	
	mysql> select * from dept;
	+--------+------------+----------+
	| DEPTNO | DNAME      | LOC      |
	+--------+------------+----------+
	|     10 | ACCOUNTING | NEW YORK |
	|     20 | RESEARCH   | DALLAS   |
	|     30 | SALES      | CHICAGO  |
	|     40 | OPERATIONS | BOSTON   |
	+--------+------------+----------+
	
	mysql> select * from salgrade;
	+-------+-------+-------+
	| GRADE | LOSAL | HISAL |
	+-------+-------+-------+
	|     1 |   700 |  1200 |
	|     2 |  1201 |  1400 |
	|     3 |  1401 |  2000 |
	|     4 |  2001 |  3000 |
	|     5 |  3001 |  9999 |
	+-------+-------+-------+

### 12、不看表中的数据，只看表的结构，有一个命令：

​	**desc 表名;**
mysql> desc dept;
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| DEPTNO | int(2)      | NO   | PRI | NULL    |       |部门编号
| DNAME  | varchar(14) | YES  |     | NULL    |       |部门名字
| LOC    | varchar(13) | YES  |     | NULL    |       |地理位置
+--------+-------------+------+-----+---------+-------+
mysql> desc emp;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| EMPNO    | int(4)      | NO   | PRI | NULL    |       |员工编号
| ENAME    | varchar(10) | YES  |     | NULL    |       |员工姓名
| JOB      | varchar(9)  | YES  |     | NULL    |       |工作岗位
| MGR      | int(4)      | YES  |     | NULL    |       |上级编号
| HIREDATE | date        | YES  |     | NULL    |       |入职日期
| SAL      | double(7,2) | YES  |     | NULL    |       |工资
| COMM     | double(7,2) | YES  |     | NULL    |       |补助
| DEPTNO   | int(2)      | YES  |     | NULL    |       |部门编号
+----------+-------------+------+-----+---------+-------+
mysql> desc salgrade;
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| GRADE | int(11) | YES  |     | NULL    |       |工资等级
| LOSAL | int(11) | YES  |     | NULL    |       |最低工资
| HISAL | int(11) | YES  |     | NULL    |       |最高工资
+-------+---------+------+-----+---------+-------+

describe缩写为：desc
mysql> describe dept;
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| DEPTNO | int(2)      | NO   | PRI | NULL    |       |
| DNAME  | varchar(14) | YES  |     | NULL    |       |
| LOC    | varchar(13) | YES  |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+

### 13、简单查询

​	13.1、查询一个字段？
​		select 字段名 from 表名;
​		其中要注意：
​			select和from都是关键字。
​			字段名和表名都是标识符。
​		

		强调：
			对于SQL语句来说，是通用的，
			所有的SQL语句以“;”结尾。
			另外SQL语句不区分大小写，都行。
		
		查询部门名字？
			mysql> select dname from dept;
			+------------+
			| dname      |
			+------------+
			| ACCOUNTING |
			| RESEARCH   |
			| SALES      |
			| OPERATIONS |
			+------------+
			4 rows in set (0.00 sec)
	
			mysql> SELECT DNAME FROM DEPT;
			+------------+
			| DNAME      |
			+------------+
			| ACCOUNTING |
			| RESEARCH   |
			| SALES      |
			| OPERATIONS |
			+------------+
			4 rows in set (0.00 sec)
	
	13.2、查询两个字段，或者多个字段怎么办？
		使用逗号隔开“,”
		查询部门编号和部门名？
			select deptno,dname from dept;
			+--------+------------+
			| deptno | dname      |
			+--------+------------+
			|     10 | ACCOUNTING |
			|     20 | RESEARCH   |
			|     30 | SALES      |
			|     40 | OPERATIONS |
			+--------+------------+
	
	13.3、查询所有字段怎么办？
	
		第一种方式：可以把每个字段都写上
			select a,b,c,d,e,f... from tablename;
	
		第二种方式：可以使用*
			select * from dept;
			+--------+------------+----------+
			| DEPTNO | DNAME      | LOC      |
			+--------+------------+----------+
			|     10 | ACCOUNTING | NEW YORK |
			|     20 | RESEARCH   | DALLAS   |
			|     30 | SALES      | CHICAGO  |
			|     40 | OPERATIONS | BOSTON   |
			+--------+------------+----------+
	
			这种方式的缺点：
				1、效率低
				2、可读性差。
			在实际开发中不建议，可以自己玩没问题。
			你可以在DOS命令窗口中想快速的看一看全表数据可以采用这种方式。
	
	13.4、给查询的列起别名？
		mysql> select deptno,dname as deptname from dept;
		+--------+------------+
		| deptno | deptname   |
		+--------+------------+
		|     10 | ACCOUNTING |
		|     20 | RESEARCH   |
		|     30 | SALES      |
		|     40 | OPERATIONS |
		+--------+------------+
		使用as关键字起别名。
		注意：只是将显示的查询结果列名显示为deptname，原表列名还是叫：dname
		记住：select语句是永远都不会进行修改操作的。（因为只负责查询）
	
		as关键字可以省略吗？可以的
			mysql> select deptno,dname deptname from dept;
		
		假设起别名的时候，别名里面有空格，怎么办？
			mysql> select deptno,dname dept name from dept;
			DBMS看到这样的语句，进行SQL语句的编译，不符合语法，编译报错。
			怎么解决？
				select deptno,dname 'dept name' from dept; //加单引号
				select deptno,dname "dept name" from dept; //加双引号
				+--------+------------+
				| deptno | dept name  |
				+--------+------------+
				|     10 | ACCOUNTING |
				|     20 | RESEARCH   |
				|     30 | SALES      |
				|     40 | OPERATIONS |
				+--------+------------+
			
			注意：在所有的数据库当中，字符串统一使用单引号括起来，
			单引号是标准，双引号在oracle数据库中用不了。但是在mysql
			中可以使用。
	
			再次强调：数据库中的字符串都是采用单引号括起来。这是标准的。
			双引号不标准。
	
	13.5、计算员工年薪？sal * 12
		mysql> select ename,sal from emp;
		+--------+---------+
		| ename  | sal     |
		+--------+---------+
		| SMITH  |  800.00 |
		| ALLEN  | 1600.00 |
		| WARD   | 1250.00 |
		| JONES  | 2975.00 |
		| MARTIN | 1250.00 |
		| BLAKE  | 2850.00 |
		| CLARK  | 2450.00 |
		| SCOTT  | 3000.00 |
		| KING   | 5000.00 |
		| TURNER | 1500.00 |
		| ADAMS  | 1100.00 |
		| JAMES  |  950.00 |
		| FORD   | 3000.00 |
		| MILLER | 1300.00 |
		+--------+---------+
		mysql> select ename,sal*12 from emp; // 结论：字段可以使用数学表达式！
		+--------+----------+
		| ename  | sal*12   |
		+--------+----------+
		| SMITH  |  9600.00 |
		| ALLEN  | 19200.00 |
		| WARD   | 15000.00 |
		| JONES  | 35700.00 |
		| MARTIN | 15000.00 |
		| BLAKE  | 34200.00 |
		| CLARK  | 29400.00 |
		| SCOTT  | 36000.00 |
		| KING   | 60000.00 |
		| TURNER | 18000.00 |
		| ADAMS  | 13200.00 |
		| JAMES  | 11400.00 |
		| FORD   | 36000.00 |
		| MILLER | 15600.00 |
		+--------+----------+
	
		mysql> select ename,sal*12 as yearsal from emp; //起别名
		+--------+----------+
		| ename  | yearsal  |
		+--------+----------+
		| SMITH  |  9600.00 |
		| ALLEN  | 19200.00 |
		| WARD   | 15000.00 |
		| JONES  | 35700.00 |
		| MARTIN | 15000.00 |
		| BLAKE  | 34200.00 |
		| CLARK  | 29400.00 |
		| SCOTT  | 36000.00 |
		| KING   | 60000.00 |
		| TURNER | 18000.00 |
		| ADAMS  | 13200.00 |
		| JAMES  | 11400.00 |
		| FORD   | 36000.00 |
		| MILLER | 15600.00 |
		+--------+----------+
	
		mysql> select ename,sal*12 as '年薪' from emp; //别名是中文，用单引号括起来。
		+--------+----------+
		| ename  | 年薪        |
		+--------+----------+
		| SMITH  |  9600.00 |
		| ALLEN  | 19200.00 |
		| WARD   | 15000.00 |
		| JONES  | 35700.00 |
		| MARTIN | 15000.00 |
		| BLAKE  | 34200.00 |
		| CLARK  | 29400.00 |
		| SCOTT  | 36000.00 |
		| KING   | 60000.00 |
		| TURNER | 18000.00 |
		| ADAMS  | 13200.00 |
		| JAMES  | 11400.00 |
		| FORD   | 36000.00 |
		| MILLER | 15600.00 |
		+--------+----------+

### 14、条件查询

14.1、什么是条件查询？
	不是将表中所有数据都查出来。是查询出来符合条件的。
	语法格式：
		select
			字段1,字段2,字段3....
		from 
			表名
		where
			条件;

14.2、都有哪些条件？

	= 等于
	查询薪资等于800的员工姓名和编号？
		select empno,ename from emp where sal = 800;
	查询SMITH的编号和薪资？
		select empno,sal from emp where ename = 'SMITH'; //字符串使用单引号
	
	<>或!= 不等于
	查询薪资不等于800的员工姓名和编号？
		select empno,ename from emp where sal != 800;
		select empno,ename from emp where sal <> 800; // 小于号和大于号组成的不等号
	
	< 小于
	查询薪资小于2000的员工姓名和编号？
		mysql> select empno,ename,sal from emp where sal < 2000;
		+-------+--------+---------+
		| empno | ename  | sal     |
		+-------+--------+---------+
		|  7369 | SMITH  |  800.00 |
		|  7499 | ALLEN  | 1600.00 |
		|  7521 | WARD   | 1250.00 |
		|  7654 | MARTIN | 1250.00 |
		|  7844 | TURNER | 1500.00 |
		|  7876 | ADAMS  | 1100.00 |
		|  7900 | JAMES  |  950.00 |
		|  7934 | MILLER | 1300.00 |
		+-------+--------+---------+


	<= 小于等于
	查询薪资小于等于3000的员工姓名和编号？
		select empno,ename,sal from emp where sal <= 3000;


	> 大于
	查询薪资大于3000的员工姓名和编号？
		select empno,ename,sal from emp where sal > 3000;
	
	>= 大于等于
	查询薪资大于等于3000的员工姓名和编号？
		select empno,ename,sal from emp where sal >= 3000;
	
	between … and …. 两个值之间, 等同于 >= and <=
	查询薪资在2450和3000之间的员工信息？包括2450和3000
		第一种方式：>= and <= （and是并且的意思。）
			select empno,ename,sal from emp where sal >= 2450 and sal <= 3000;
			+-------+-------+---------+
			| empno | ename | sal     |
			+-------+-------+---------+
			|  7566 | JONES | 2975.00 |
			|  7698 | BLAKE | 2850.00 |
			|  7782 | CLARK | 2450.00 |
			|  7788 | SCOTT | 3000.00 |
			|  7902 | FORD  | 3000.00 |
			+-------+-------+---------+
		第二种方式：between … and …
			select 
				empno,ename,sal 
			from 
				emp 
			where 
				sal between 2450 and 3000;
			
			注意：
				使用between and的时候，必须遵循左小右大。
				between and是闭区间，包括两端的值。
	
	is null 为 null（is not null 不为空）
	查询哪些员工的津贴/补助为null？
		mysql> select empno,ename,sal,comm from emp where comm = null;
		Empty set (0.00 sec)
	
		mysql> select empno,ename,sal,comm from emp where comm is null;
		+-------+--------+---------+------+
		| empno | ename  | sal     | comm |
		+-------+--------+---------+------+
		|  7369 | SMITH  |  800.00 | NULL |
		|  7566 | JONES  | 2975.00 | NULL |
		|  7698 | BLAKE  | 2850.00 | NULL |
		|  7782 | CLARK  | 2450.00 | NULL |
		|  7788 | SCOTT  | 3000.00 | NULL |
		|  7839 | KING   | 5000.00 | NULL |
		|  7876 | ADAMS  | 1100.00 | NULL |
		|  7900 | JAMES  |  950.00 | NULL |
		|  7902 | FORD   | 3000.00 | NULL |
		|  7934 | MILLER | 1300.00 | NULL |
		+-------+--------+---------+------+
		10 rows in set (0.00 sec)
	
		注意：在数据库当中null不能使用等号进行衡量。需要使用is null
		因为数据库中的null代表什么也没有，它不是一个值，所以不能使用
		等号衡量。
	
	查询哪些员工的津贴/补助不为null？
		select empno,ename,sal,comm from emp where comm is not null;
		+-------+--------+---------+---------+
		| empno | ename  | sal     | comm    |
		+-------+--------+---------+---------+
		|  7499 | ALLEN  | 1600.00 |  300.00 |
		|  7521 | WARD   | 1250.00 |  500.00 |
		|  7654 | MARTIN | 1250.00 | 1400.00 |
		|  7844 | TURNER | 1500.00 |    0.00 |
		+-------+--------+---------+---------+
	
	and 并且
	查询工作岗位是MANAGER并且工资大于2500的员工信息？
		select 
			empno,ename,job,sal 
		from 
			emp 
		where 
			job = 'MANAGER' and sal > 2500;
		
		+-------+-------+---------+---------+
		| empno | ename | job     | sal     |
		+-------+-------+---------+---------+
		|  7566 | JONES | MANAGER | 2975.00 |
		|  7698 | BLAKE | MANAGER | 2850.00 |
		+-------+-------+---------+---------+
	
	or 或者
	查询工作岗位是MANAGER和SALESMAN的员工？
		select empno,ename,job from emp where job = 'MANAGER';
		select empno,ename,job from emp where job = 'SALESMAN';
	
		select 
			empno,ename,job
		from
			emp
		where 
			job = 'MANAGER' or job = 'SALESMAN';
		
		+-------+--------+----------+
		| empno | ename  | job      |
		+-------+--------+----------+
		|  7499 | ALLEN  | SALESMAN |
		|  7521 | WARD   | SALESMAN |
		|  7566 | JONES  | MANAGER  |
		|  7654 | MARTIN | SALESMAN |
		|  7698 | BLAKE  | MANAGER  |
		|  7782 | CLARK  | MANAGER  |
		|  7844 | TURNER | SALESMAN |
		+-------+--------+----------+
	
	and和or同时出现的话，有优先级问题吗？
	查询工资大于2500，并且部门编号为10或20部门的员工？
		select 
			*
		from
			emp
		where
			sal > 2500 and deptno = 10 or deptno = 20;
		分析以上语句的问题？
			and优先级比or高。
			以上语句会先执行and，然后执行or。
			以上这个语句表示什么含义？
				找出工资大于2500并且部门编号为10的员工，或者20部门所有员工找出来。
		
		select 
			*
		from
			emp
		where
			sal > 2500 and (deptno = 10 or deptno = 20);
		
		and和or同时出现，and优先级较高。如果想让or先执行，需要加“小括号”
		以后在开发中，如果不确定优先级，就加小括号就行了。
	
	in 包含，相当于多个 or （not in 不在这个范围中）
		查询工作岗位是MANAGER和SALESMAN的员工？
			select empno,ename,job from emp where job = 'MANAGER' or job = 'SALESMAN';
			select empno,ename,job from emp where job in('MANAGER', 'SALESMAN');
			+-------+--------+----------+
			| empno | ename  | job      |
			+-------+--------+----------+
			|  7499 | ALLEN  | SALESMAN |
			|  7521 | WARD   | SALESMAN |
			|  7566 | JONES  | MANAGER  |
			|  7654 | MARTIN | SALESMAN |
			|  7698 | BLAKE  | MANAGER  |
			|  7782 | CLARK  | MANAGER  |
			|  7844 | TURNER | SALESMAN |
			+-------+--------+----------+
			注意：in不是一个区间。in后面跟的是具体的值。
	
		查询薪资是800和5000的员工信息？
			select ename,sal from emp where sal = 800 or sal = 5000;
			select ename,sal from emp where sal in(800, 5000); //这个不是表示800到5000都找出来。
			+-------+---------+
			| ename | sal     |
			+-------+---------+
			| SMITH |  800.00 |
			| KING  | 5000.00 |
			+-------+---------+
			select ename,sal from emp where sal in(800, 5000, 3000);
	
			// not in 表示不在这几个值当中的数据。
			select ename,sal from emp where sal not in(800, 5000, 3000);
			+--------+---------+
			| ename  | sal     |
			+--------+---------+
			| ALLEN  | 1600.00 |
			| WARD   | 1250.00 |
			| JONES  | 2975.00 |
			| MARTIN | 1250.00 |
			| BLAKE  | 2850.00 |
			| CLARK  | 2450.00 |
			| TURNER | 1500.00 |
			| ADAMS  | 1100.00 |
			| JAMES  |  950.00 |
			| MILLER | 1300.00 |
			+--------+---------+
	
	not 可以取非，主要用在 is 或 in 中
		is null
		is not null
		in
		not in
	
	like 
		称为模糊查询，支持%或下划线匹配
		%匹配任意多个字符
		下划线：任意一个字符。
		（%是一个特殊的符号，_ 也是一个特殊符号）
	
		找出名字中含有O的？
		mysql> select ename from emp where ename like '%O%';
		+-------+
		| ename |
		+-------+
		| JONES |
		| SCOTT |
		| FORD  |
		+-------+
	
		找出名字以T结尾的？
			select ename from emp where ename like '%T';
			
		找出名字以K开始的？
			select ename from emp where ename like 'K%';
	
		找出第二个字每是A的？
			select ename from emp where ename like '_A%';
		
		找出第三个字母是R的？
			select ename from emp where ename like '__R%';
		
		t_student学生表
		name字段
		----------------------
		zhangsan
		lisi
		wangwu
		zhaoliu
		jack_son
	
		找出名字中有“_”的？
			select name from t_student where name like '%_%'; //这样不行。
	
			mysql> select name from t_student where name like '%\_%'; // \转义字符。
			+----------+
			| name     |
			+----------+
			| jack_son |
			+----------+

### 15、排序

15.1、查询所有员工薪资，排序？
	select 
		ename,sal
	from
		emp
	order by
		sal; // 默认是升序！！！

	+--------+---------+
	| ename  | sal     |
	+--------+---------+
	| SMITH  |  800.00 |
	| JAMES  |  950.00 |
	| ADAMS  | 1100.00 |
	| WARD   | 1250.00 |
	| MARTIN | 1250.00 |
	| MILLER | 1300.00 |
	| TURNER | 1500.00 |
	| ALLEN  | 1600.00 |
	| CLARK  | 2450.00 |
	| BLAKE  | 2850.00 |
	| JONES  | 2975.00 |
	| FORD   | 3000.00 |
	| SCOTT  | 3000.00 |
	| KING   | 5000.00 |
	+--------+---------+

15.2、怎么降序？

	指定降序：
	select 
		ename,sal
	from
		emp
	order by
		sal desc;

+--------+---------+
| ename  | sal     |
+--------+---------+
| KING   | 5000.00 |
| SCOTT  | 3000.00 |
| FORD   | 3000.00 |
| JONES  | 2975.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
| MARTIN | 1250.00 |
| WARD   | 1250.00 |
| ADAMS  | 1100.00 |
| JAMES  |  950.00 |
| SMITH  |  800.00 |
+--------+---------+

	指定升序？
	select 
		ename,sal
	from
		emp
	order by
		sal asc;

+--------+---------+
| ename  | sal     |
+--------+---------+
| SMITH  |  800.00 |
| JAMES  |  950.00 |
| ADAMS  | 1100.00 |
| WARD   | 1250.00 |
| MARTIN | 1250.00 |
| MILLER | 1300.00 |
| TURNER | 1500.00 |
| ALLEN  | 1600.00 |
| CLARK  | 2450.00 |
| BLAKE  | 2850.00 |
| JONES  | 2975.00 |
| FORD   | 3000.00 |
| SCOTT  | 3000.00 |
| KING   | 5000.00 |
+--------+---------+

15.3、可以两个字段排序吗？或者说按照多个字段排序？
	查询员工名字和薪资，要求按照薪资升序，如果薪资一样的话，
	再按照名字升序排列。
	select 
		ename,sal
	from
		emp
	order by
		sal asc, ename asc; // sal在前，起主导，只有sal相等的时候，才会考虑启用ename排序。

	+--------+---------+
	| ename  | sal     |
	+--------+---------+
	| SMITH  |  800.00 |
	| JAMES  |  950.00 |
	| ADAMS  | 1100.00 |
	| MARTIN | 1250.00 |
	| WARD   | 1250.00 |
	| MILLER | 1300.00 |
	| TURNER | 1500.00 |
	| ALLEN  | 1600.00 |
	| CLARK  | 2450.00 |
	| BLAKE  | 2850.00 |
	| JONES  | 2975.00 |
	| FORD   | 3000.00 |
	| SCOTT  | 3000.00 |
	| KING   | 5000.00 |
	+--------+---------+

15.4、了解：根据字段的位置也可以排序
	select ename,sal from emp order by 2; // 2表示第二列。第二列是sal
	按照查询结果的第2列sal排序。

	了解一下，不建议在开发中这样写，因为不健壮。
	因为列的顺序很容易发生改变，列顺序修改之后，2就废了。

### 16、综合案例

​	找出工资在1250到3000之间的员工信息，要求按照薪资降序排列。
​	select 
​		ename,sal
​	from
​		emp
​	where
​		sal between 1250 and 3000
​	order by
​		sal desc;

+--------+---------+
| ename  | sal     |
+--------+---------+
| FORD   | 3000.00 |
| SCOTT  | 3000.00 |
| JONES  | 2975.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
| MARTIN | 1250.00 |
| WARD   | 1250.00 |
+--------+---------+
	
	关键字顺序不能变：
		select
			...
		from
			...
		where
			...
		order by
			...
		
		以上语句的执行顺序必须掌握：
			第一步：from
			第二步：where
			第三步：select
			第四步：order by（排序总是在最后执行！）

### 17、数据处理函数

17.1、数据处理函数又被称为单行处理函数

	单行处理函数的特点：一个输入对应一个输出。
	
	和单行处理函数相对的是：多行处理函数。（多行处理函数特点：多个输入，对应1个输出！）

17.2、单行处理函数常见的有哪些？

	lower 转换小写
		mysql> select lower(ename) as ename from emp;
		+--------+
		| ename  |
		+--------+
		| smith  |
		| allen  |
		| ward   |
		| jones  |
		| martin |
		| blake  |
		| clark  |
		| scott  |
		| king   |
		| turner |
		| adams  |
		| james  |
		| ford   |
		| miller |
		+--------+
		14个输入，最后还是14个输出。这是单行处理函数的特点。
	
	upper 转换大写
		mysql> select * from t_student;
		+----------+
		| name     |
		+----------+
		| zhangsan |
		| lisi     |
		| wangwu   |
		| jack_son |
		+----------+
	
		mysql> select upper(name) as name from t_student;
		+----------+
		| name     |
		+----------+
		| ZHANGSAN |
		| LISI     |
		| WANGWU   |
		| JACK_SON |
		+----------+
	
	substr 取子串（substr( 被截取的字符串, 起始下标,截取的长度)）
		select substr(ename, 1, 1) as ename from emp;
		注意：起始下标从1开始，没有0.
		找出员工名字第一个字母是A的员工信息？
			第一种方式：模糊查询
				select ename from emp where ename like 'A%';
			第二种方式：substr函数
				select 
					ename 
				from 
					emp 
				where 
					substr(ename,1,1) = 'A';
	
		首字母大写？
			select name from t_student;
			select upper(substr(name,1,1)) from t_student;
			select substr(name,2,length(name) - 1) from t_student;
			select concat(upper(substr(name,1,1)),substr(name,2,length(name) - 1)) as result from t_student;
			+----------+
			| result   |
			+----------+
			| Zhangsan |
			| Lisi     |
			| Wangwu   |
			| Jack_son |
			+----------+
		
	concat函数进行字符串的拼接
		select concat(empno,ename) from emp;
		+---------------------+
		| concat(empno,ename) |
		+---------------------+
		| 7369SMITH           |
		| 7499ALLEN           |
		| 7521WARD            |
		| 7566JONES           |
		| 7654MARTIN          |
		| 7698BLAKE           |
		| 7782CLARK           |
		| 7788SCOTT           |
		| 7839KING            |
		| 7844TURNER          |
		| 7876ADAMS           |
		| 7900JAMES           |
		| 7902FORD            |
		| 7934MILLER          |
		+---------------------+
	
	length 取长度
		select length(ename) enamelength from emp;
		+-------------+
		| enamelength |
		+-------------+
		|           5 |
		|           5 |
		|           4 |
		|           5 |
		|           6 |
		|           5 |
		|           5 |
		|           5 |
		|           4 |
		|           6 |
		|           5 |
		|           5 |
		|           4 |
		|           6 |
		+-------------+
	
	trim 去空格
		mysql> select * from emp where ename = '  KING';
		Empty set (0.00 sec)
	
		mysql> select * from emp where ename = trim('   KING');
		+-------+-------+-----------+------+------------+---------+------+--------+
		| EMPNO | ENAME | JOB       | MGR  | HIREDATE   | SAL     | COMM | DEPTNO |
		+-------+-------+-----------+------+------------+---------+------+--------+
		|  7839 | KING  | PRESIDENT | NULL | 1981-11-17 | 5000.00 | NULL |     10 |
		+-------+-------+-----------+------+------------+---------+------+--------+
	
	str_to_date 将字符串转换成日期
	date_format 格式化日期
	format 设置千分位
	
	case..when..then..when..then..else..end
		当员工的工作岗位是MANAGER的时候，工资上调10%，当工作岗位是SALESMAN的时候，工资上调50%,其它正常。
		（注意：不修改数据库，只是将查询结果显示为工资上调）
		select 
			ename,
			job, 
			sal as oldsal,
			(case job when 'MANAGER' then sal*1.1 when 'SALESMAN' then sal*1.5 else sal end) as newsal 
		from 
			emp;
		
		+--------+-----------+---------+---------+
		| ename  | job       | oldsal  | newsal  |
		+--------+-----------+---------+---------+
		| SMITH  | CLERK     |  800.00 |  800.00 |
		| ALLEN  | SALESMAN  | 1600.00 | 2400.00 |
		| WARD   | SALESMAN  | 1250.00 | 1875.00 |
		| JONES  | MANAGER   | 2975.00 | 3272.50 |
		| MARTIN | SALESMAN  | 1250.00 | 1875.00 |
		| BLAKE  | MANAGER   | 2850.00 | 3135.00 |
		| CLARK  | MANAGER   | 2450.00 | 2695.00 |
		| SCOTT  | ANALYST   | 3000.00 | 3000.00 |
		| KING   | PRESIDENT | 5000.00 | 5000.00 |
		| TURNER | SALESMAN  | 1500.00 | 2250.00 |
		| ADAMS  | CLERK     | 1100.00 | 1100.00 |
		| JAMES  | CLERK     |  950.00 |  950.00 |
		| FORD   | ANALYST   | 3000.00 | 3000.00 |
		| MILLER | CLERK     | 1300.00 | 1300.00 |
		+--------+-----------+---------+---------+
	
	round 四舍五入
		select 字段 from 表名;
		select ename from emp;
		select 'abc' from emp; // select后面直接跟“字面量/字面值”
	
		mysql> select 'abc' as bieming from emp;
		+---------+
		| bieming |
		+---------+
		| abc     |
		| abc     |
		| abc     |
		| abc     |
		| abc     |
		| abc     |
		| abc     |
		| abc     |
		| abc     |
		| abc     |
		| abc     |
		| abc     |
		| abc     |
		| abc     |
		+---------+
	
		mysql> select abc from emp;
		ERROR 1054 (42S22): Unknown column 'abc' in 'field list'
		这样肯定报错，因为会把abc当做一个字段的名字，去emp表中找abc字段去了。
	
		select 1000 as num from emp; // 1000 也是被当做一个字面量/字面值。
		+------+
		| num  |
		+------+
		| 1000 |
		| 1000 |
		| 1000 |
		| 1000 |
		| 1000 |
		| 1000 |
		| 1000 |
		| 1000 |
		| 1000 |
		| 1000 |
		| 1000 |
		| 1000 |
		| 1000 |
		| 1000 |
		+------+
	
		结论：select后面可以跟某个表的字段名（可以等同看做变量名），也可以跟字面量/字面值（数据）。
		select 21000 as num from dept;
		+-------+
		| num   |
		+-------+
		| 21000 |
		| 21000 |
		| 21000 |
		| 21000 |
		+-------+
	
		mysql> select round(1236.567, 0) as result from emp; //保留整数位。
		+--------+
		| result |
		+--------+
		|   1237 |
		|   1237 |
		|   1237 |
		|   1237 |
		|   1237 |
		|   1237 |
		|   1237 |
		|   1237 |
		|   1237 |
		|   1237 |
		|   1237 |
		|   1237 |
		|   1237 |
		|   1237 |
		+--------+
	
		select round(1236.567, 1) as result from emp; //保留1个小数
		select round(1236.567, 2) as result from emp; //保留2个小数
		select round(1236.567, -1) as result from emp; // 保留到十位。
		+--------+
		| result |
		+--------+
		|   1240 |
		|   1240 |
		|   1240 |
		|   1240 |
		|   1240 |
		|   1240 |
		|   1240 |
		|   1240 |
		|   1240 |
		|   1240 |
		|   1240 |
		|   1240 |
		|   1240 |
		|   1240 |
		+--------+
	
		select round(1236.567, -2) as result from emp;
		+--------+
		| result |
		+--------+
		|   1200 |
		|   1200 |
		|   1200 |
		|   1200 |
		|   1200 |
		|   1200 |
		|   1200 |
		|   1200 |
		|   1200 |
		|   1200 |
		|   1200 |
		|   1200 |
		|   1200 |
		|   1200 |
		+--------+
	
	rand() 生成随机数
		mysql> select round(rand()*100,0) from emp; // 100以内的随机数
		+---------------------+
		| round(rand()*100,0) |
		+---------------------+
		|                  76 |
		|                  29 |
		|                  15 |
		|                  88 |
		|                  95 |
		|                   9 |
		|                  63 |
		|                  89 |
		|                  54 |
		|                   3 |
		|                  54 |
		|                  61 |
		|                  42 |
		|                  28 |
		+---------------------+
		
	ifnull 可以将 null 转换成一个具体值
		ifnull是空处理函数。专门处理空的。
		在所有数据库当中，只要有NULL参与的数学运算，最终结果就是NULL。
		mysql> select ename, sal + comm as salcomm from emp;
		+--------+---------+
		| ename  | salcomm |
		+--------+---------+
		| SMITH  |    NULL |
		| ALLEN  | 1900.00 |
		| WARD   | 1750.00 |
		| JONES  |    NULL |
		| MARTIN | 2650.00 |
		| BLAKE  |    NULL |
		| CLARK  |    NULL |
		| SCOTT  |    NULL |
		| KING   |    NULL |
		| TURNER | 1500.00 |
		| ADAMS  |    NULL |
		| JAMES  |    NULL |
		| FORD   |    NULL |
		| MILLER |    NULL |
		+--------+---------+
	
		计算每个员工的年薪？
			年薪 = (月薪 + 月补助) * 12
			
			select ename, (sal + comm) * 12 as yearsal from emp;
			+--------+----------+
			| ename  | yearsal  |
			+--------+----------+
			| SMITH  |     NULL |
			| ALLEN  | 22800.00 |
			| WARD   | 21000.00 |
			| JONES  |     NULL |
			| MARTIN | 31800.00 |
			| BLAKE  |     NULL |
			| CLARK  |     NULL |
			| SCOTT  |     NULL |
			| KING   |     NULL |
			| TURNER | 18000.00 |
			| ADAMS  |     NULL |
			| JAMES  |     NULL |
			| FORD   |     NULL |
			| MILLER |     NULL |
			+--------+----------+
	
			注意：NULL只要参与运算，最终结果一定是NULL。为了避免这个现象，需要使用ifnull函数。
			ifnull函数用法：ifnull(数据, 被当做哪个值)
				如果“数据”为NULL的时候，把这个数据结构当做哪个值。
			
			补助为NULL的时候，将补助当做0
				select ename, (sal + ifnull(comm, 0)) * 12 as yearsal from emp;
				+--------+----------+
				| ename  | yearsal  |
				+--------+----------+
				| SMITH  |  9600.00 |
				| ALLEN  | 22800.00 |
				| WARD   | 21000.00 |
				| JONES  | 35700.00 |
				| MARTIN | 31800.00 |
				| BLAKE  | 34200.00 |
				| CLARK  | 29400.00 |
				| SCOTT  | 36000.00 |
				| KING   | 60000.00 |
				| TURNER | 18000.00 |
				| ADAMS  | 13200.00 |
				| JAMES  | 11400.00 |
				| FORD   | 36000.00 |
				| MILLER | 15600.00 |
				+--------+----------+

### 18、分组函数（多行处理函数）

​	

	多行处理函数的特点：输入多行，最终输出一行。
	
	5个：
		count	计数
		sum	求和
		avg	平均值
		max	最大值
		min	最小值
	
	注意：
		分组函数在使用的时候必须先进行分组，然后才能用。
		如果你没有对数据进行分组，整张表默认为一组。
	
	找出最高工资？
		mysql> select max(sal) from emp;
		+----------+
		| max(sal) |
		+----------+
		|  5000.00 |
		+----------+
	
	找出最低工资？
		mysql> select min(sal) from emp;
		+----------+
		| min(sal) |
		+----------+
		|   800.00 |
		+----------+
	
	计算工资和：
		mysql> select sum(sal) from emp;
		+----------+
		| sum(sal) |
		+----------+
		| 29025.00 |
		+----------+
	
	计算平均工资：
		mysql> select avg(sal) from emp;
		+-------------+
		| avg(sal)    |
		+-------------+
		| 2073.214286 |
		+-------------+
		14个工资全部加起来，然后除以14。
	
	计算员工数量？
		mysql> select count(ename) from emp;
		+--------------+
		| count(ename) |
		+--------------+
		|           14 |
		+--------------+
	
	分组函数在使用的时候需要注意哪些？
	
		第一点：分组函数自动忽略NULL，你不需要提前对NULL进行处理。
		mysql> select sum(comm) from emp;
		+-----------+
		| sum(comm) |
		+-----------+
		|   2200.00 |
		+-----------+
		
		mysql> select count(comm) from emp;
		+-------------+
		| count(comm) |
		+-------------+
		|           4 |
		+-------------+
		mysql> select avg(comm) from emp;
		+------------+
		| avg(comm)  |
		+------------+
		| 550.000000 |
		+------------+
	
		第二点：分组函数中count(*)和count(具体字段)有什么区别？
			mysql> select count(*) from emp;
			+----------+
			| count(*) |
			+----------+
			|       14 |
			+----------+
	
			mysql> select count(comm) from emp;
			+-------------+
			| count(comm) |
			+-------------+
			|           4 |
			+-------------+
	
			count(具体字段)：表示统计该字段下所有不为NULL的元素的总数。
			count(*)：统计表当中的总行数。（只要有一行数据count则++）
						因为每一行记录不可能都为NULL，一行数据中有一列不为NULL，则这行数据就是有效的。
		
		第三点：分组函数不能够直接使用在where子句中。
			找出比最低工资高的员工信息。
				select ename,sal from emp where sal > min(sal);
				表面上没问题，运行一下？
					ERROR 1111 (HY000): Invalid use of group function
		?????????????????????????????????????????????????????????????????????
			说完分组查询(group by)之后就明白了了。
	
		第四点：所有的分组函数可以组合起来一起用。
			select sum(sal),min(sal),max(sal),avg(sal),count(*) from emp;
			+----------+----------+----------+-------------+----------+
			| sum(sal) | min(sal) | max(sal) | avg(sal)    | count(*) |
			+----------+----------+----------+-------------+----------+
			| 29025.00 |   800.00 |  5000.00 | 2073.214286 |       14 |
			+----------+----------+----------+-------------+----------+

### 19、分组查询（非常重要：五颗星*****）

​	

	19.1、什么是分组查询？
		在实际的应用中，可能有这样的需求，需要先进行分组，然后对每一组的数据进行操作。
		这个时候我们需要使用分组查询，怎么进行分组查询呢？
			select
				...
			from
				...
			group by
				...
			
			计算每个部门的工资和？
			计算每个工作岗位的平均薪资？
			找出每个工作岗位的最高薪资？
			....
	
	19.2、将之前的关键字全部组合在一起，来看一下他们的执行顺序？
		select
			...
		from
			...
		where
			...
		group by
			...
		order by
			...
		
		以上关键字的顺序不能颠倒，需要记忆。
		执行顺序是什么？
			1. from
			2. where
			3. group by
			4. select
			5. order by
		
		为什么分组函数不能直接使用在where后面？
			select ename,sal from emp where sal > min(sal);//报错。
			因为分组函数在使用的时候必须先分组之后才能使用。
			where执行的时候，还没有分组。所以where后面不能出现分组函数。
	
			select sum(sal) from emp; 
			这个没有分组，为啥sum()函数可以用呢？
				因为select在group by之后执行。
			
	19.3、找出每个工作岗位的工资和？
	
		实现思路：按照工作岗位分组，然后对工资求和。
			select 
				job,sum(sal)
			from
				emp
			group by
				job;
			
			+-----------+----------+
			| job       | sum(sal) |
			+-----------+----------+
			| ANALYST   |  6000.00 |
			| CLERK     |  4150.00 |
			| MANAGER   |  8275.00 |
			| PRESIDENT |  5000.00 |
			| SALESMAN  |  5600.00 |
			+-----------+----------+
			以上这个语句的执行顺序？
				先从emp表中查询数据。
				根据job字段进行分组。
				然后对每一组的数据进行sum(sal)
		
		select ename,job,sum(sal) from emp group by job;
		+-------+-----------+----------+
		| ename | job       | sum(sal) |
		+-------+-----------+----------+
		| SCOTT | ANALYST   |  6000.00 |
		| SMITH | CLERK     |  4150.00 |
		| JONES | MANAGER   |  8275.00 |
		| KING  | PRESIDENT |  5000.00 |
		| ALLEN | SALESMAN  |  5600.00 |
		+-------+-----------+----------+
		以上语句在mysql中可以执行，但是毫无意义。
		以上语句在oracle中执行报错。
		oracle的语法比mysql的语法严格。（mysql的语法相对来说松散一些！）
	
		重点结论：
			在一条select语句当中，如果有group by语句的话，
			select后面只能跟：参加分组的字段，以及分组函数。
			其它的一律不能跟。
	
	19.4、找出每个部门的最高薪资
		实现思路是什么？
			按照部门编号分组，求每一组的最大值。
	
			select后面添加ename字段没有意义，另外oracle会报错。
			mysql> select ename,deptno,max(sal) from emp group by deptno;
			+-------+--------+----------+
			| ename | deptno | max(sal) |
			+-------+--------+----------+
			| CLARK |     10 |  5000.00 |
			| SMITH |     20 |  3000.00 |
			| ALLEN |     30 |  2850.00 |
			+-------+--------+----------+
	
			mysql> select deptno,max(sal) from emp group by deptno;
			+--------+----------+
			| deptno | max(sal) |
			+--------+----------+
			|     10 |  5000.00 |
			|     20 |  3000.00 |
			|     30 |  2850.00 |
			+--------+----------+
	
	19.5、找出“每个部门，不同工作岗位”的最高薪资？
		+--------+-----------+---------+--------+
		| ename  | job       | sal     | deptno |
		+--------+-----------+---------+--------+
		| MILLER | CLERK     | 1300.00 |     10 |
		| KING   | PRESIDENT | 5000.00 |     10 |
		| CLARK  | MANAGER   | 2450.00 |     10 |
	
		| FORD   | ANALYST   | 3000.00 |     20 |
		| ADAMS  | CLERK     | 1100.00 |     20 |
		| SCOTT  | ANALYST   | 3000.00 |     20 |
		| JONES  | MANAGER   | 2975.00 |     20 |
		| SMITH  | CLERK     |  800.00 |     20 |
	
		| BLAKE  | MANAGER   | 2850.00 |     30 |
		| MARTIN | SALESMAN  | 1250.00 |     30 |
		| ALLEN  | SALESMAN  | 1600.00 |     30 |
		| TURNER | SALESMAN  | 1500.00 |     30 |
		| WARD   | SALESMAN  | 1250.00 |     30 |
		| JAMES  | CLERK     |  950.00 |     30 |
		+--------+-----------+---------+--------+
		技巧：两个字段联合成1个字段看。（两个字段联合分组）
		select 
			deptno, job, max(sal)
		from
			emp
		group by
			deptno, job;
	
		+--------+-----------+----------+
		| deptno | job       | max(sal) |
		+--------+-----------+----------+
		|     10 | CLERK     |  1300.00 |
		|     10 | MANAGER   |  2450.00 |
		|     10 | PRESIDENT |  5000.00 |
		|     20 | ANALYST   |  3000.00 |
		|     20 | CLERK     |  1100.00 |
		|     20 | MANAGER   |  2975.00 |
		|     30 | CLERK     |   950.00 |
		|     30 | MANAGER   |  2850.00 |
		|     30 | SALESMAN  |  1600.00 |
		+--------+-----------+----------+
		
	19.6、使用having可以对分完组之后的数据进一步过滤。
	having不能单独使用，having不能代替where，having必须
	和group by联合使用。
	
	找出每个部门最高薪资，要求显示最高薪资大于3000的？
	
		第一步：找出每个部门最高薪资
			按照部门编号分组，求每一组最大值。
			select deptno,max(sal) from emp group by deptno;
			+--------+----------+
			| deptno | max(sal) |
			+--------+----------+
			|     10 |  5000.00 |
			|     20 |  3000.00 |
			|     30 |  2850.00 |
			+--------+----------+
		
		第二步：要求显示最高薪资大于3000
			select 
				deptno,max(sal) 
			from 
				emp 
			group by 
				deptno
			having
				max(sal) > 3000;
	
			+--------+----------+
			| deptno | max(sal) |
			+--------+----------+
			|     10 |  5000.00 |
			+--------+----------+


			思考一个问题：以上的sql语句执行效率是不是低？
			比较低，实际上可以这样考虑：先将大于3000的都找出来，然后再分组。
			select 
				deptno,max(sal)
			from
				emp
			where
				sal > 3000
			group by
				deptno;
			
			+--------+----------+
			| deptno | max(sal) |
			+--------+----------+
			|     10 |  5000.00 |
			+--------+----------+
	
			优化策略：
				where和having，优先选择where，where实在完成不了了，再选择
				having。
		
		19.7、where没办法的？？？？
			找出每个部门平均薪资，要求显示平均薪资高于2500的。
	
			第一步：找出每个部门平均薪资
				select deptno,avg(sal) from emp group by deptno;
				+--------+-------------+
				| deptno | avg(sal)    |
				+--------+-------------+
				|     10 | 2916.666667 |
				|     20 | 2175.000000 |
				|     30 | 1566.666667 |
				+--------+-------------+
	
			第二步：要求显示平均薪资高于2500的
				select 
					deptno,avg(sal) 
				from 
					emp 
				group by 
					deptno
				having
					avg(sal) > 2500;
			
			+--------+-------------+
			| deptno | avg(sal)    |
			+--------+-------------+
			|     10 | 2916.666667 |
			+--------+-------------+

20、大总结（单表的查询学完了）
	select 
		...
	from
		...
	where
		...
	group by
		...
	having
		...
	order by
		...
	

	以上关键字只能按照这个顺序来，不能颠倒。
	
	执行顺序？
		1. from
		2. where
		3. group by
		4. having
		5. select
		6. order by
	
	从某张表中查询数据，
	先经过where条件筛选出有价值的数据。
	对这些有价值的数据进行分组。
	分组之后可以使用having继续筛选。
	select查询出来。
	最后排序输出！
	
	找出每个岗位的平均薪资，要求显示平均薪资大于1500的，除MANAGER岗位之外，
	要求按照平均薪资降序排。
		select 
			job, avg(sal) as avgsal
		from
			emp
		where
			job <> 'MANAGER'
		group by
			job
		having
			avg(sal) > 1500
		order by
			avgsal desc;
	
		+-----------+-------------+
		| job       | avgsal      |
		+-----------+-------------+
		| PRESIDENT | 5000.000000 |
		| ANALYST   | 3000.000000 |
		+-----------+-------------+







## day02课堂笔记

### 1、把查询结果去除重复记录【distinct】

​	注意：原表数据不会被修改，只是查询结果去重。
​	去重需要使用一个关键字：distinct

	mysql> select distinct job from emp;
	+-----------+
	| job       |
	+-----------+
	| CLERK     |
	| SALESMAN  |
	| MANAGER   |
	| ANALYST   |
	| PRESIDENT |
	+-----------+
	
	// 这样编写是错误的，语法错误。
	// distinct只能出现在所有字段的最前方。
	mysql> select ename,distinct job from emp;
	
	// distinct出现在job,deptno两个字段之前，表示两个字段联合起来去重。
	mysql> select distinct job,deptno from emp;
	+-----------+--------+
	| job       | deptno |
	+-----------+--------+
	| CLERK     |     20 |
	| SALESMAN  |     30 |
	| MANAGER   |     20 |
	| MANAGER   |     30 |
	| MANAGER   |     10 |
	| ANALYST   |     20 |
	| PRESIDENT |     10 |
	| CLERK     |     30 |
	| CLERK     |     10 |
	+-----------+--------+
	
	统计一下工作岗位的数量？
		select count(distinct job) from emp;
		+---------------------+
		| count(distinct job) |
		+---------------------+
		|                   5 |
		+---------------------+

### 2、连接查询

2.1、什么是连接查询？
	从一张表中单独查询，称为单表查询。
	emp表和dept表联合起来查询数据，从emp表中取员工名字，从dept表中取部门名字。
	这种跨表查询，多张表联合起来查询数据，被称为连接查询。

2.2、连接查询的分类？

	根据语法的年代分类：
		SQL92：1992年的时候出现的语法
		SQL99：1999年的时候出现的语法
		我们这里重点学习SQL99.(这个过程中简单演示一个SQL92的例子)
	
	根据表连接的方式分类：
		内连接：
			等值连接
			非等值连接
			自连接
	
		外连接：
			左外连接（左连接）
			右外连接（右连接）
	
		全连接（不讲）

2.3、当两张表进行连接查询时，没有任何条件的限制会发生什么现象？

	案例：查询每个员工所在部门名称？
		mysql> select ename,deptno from emp;
		+--------+--------+
		| ename  | deptno |
		+--------+--------+
		| SMITH  |     20 |
		| ALLEN  |     30 |
		| WARD   |     30 |
		| JONES  |     20 |
		| MARTIN |     30 |
		| BLAKE  |     30 |
		| CLARK  |     10 |
		| SCOTT  |     20 |
		| KING   |     10 |
		| TURNER |     30 |
		| ADAMS  |     20 |
		| JAMES  |     30 |
		| FORD   |     20 |
		| MILLER |     10 |
		+--------+--------+
		mysql> select * from dept;
		+--------+------------+----------+
		| DEPTNO | DNAME      | LOC      |
		+--------+------------+----------+
		|     10 | ACCOUNTING | NEW YORK |
		|     20 | RESEARCH   | DALLAS   |
		|     30 | SALES      | CHICAGO  |
		|     40 | OPERATIONS | BOSTON   |
		+--------+------------+----------+
	
		两张表连接没有任何条件限制：
		select ename,dname from emp, dept;
		+--------+------------+
		| ename  | dname      |
		+--------+------------+
		| SMITH  | ACCOUNTING |
		| SMITH  | RESEARCH   |
		| SMITH  | SALES      |
		| SMITH  | OPERATIONS |
		| ALLEN  | ACCOUNTING |
		| ALLEN  | RESEARCH   |
		| ALLEN  | SALES      |
		| ALLEN  | OPERATIONS |
		...
		56 rows in set (0.00 sec)
		14 * 4 = 56
	
		当两张表进行连接查询，没有任何条件限制的时候，最终查询结果条数，是
		两张表条数的乘积，这种现象被称为：笛卡尔积现象。（笛卡尔发现的，这是
		一个数学现象。）

2.4、怎么避免笛卡尔积现象？
	连接时加条件，满足这个条件的记录被筛选出来！
	select 
		ename,dname 
	from 
		emp, dept
	where
		emp.deptno = dept.deptno;
	
	select 
		emp.ename,dept.dname 
	from 
		emp, dept
	where
		emp.deptno = dept.deptno;
	
	// 表起别名。很重要。效率问题。
	select 
		e.ename,d.dname 
	from 
		emp e, dept d
	where
		e.deptno = d.deptno; //SQL92语法。
	
	+--------+------------+
	| ename  | dname      |
	+--------+------------+
	| CLARK  | ACCOUNTING |
	| KING   | ACCOUNTING |
	| MILLER | ACCOUNTING |
	| SMITH  | RESEARCH   |
	| JONES  | RESEARCH   |
	| SCOTT  | RESEARCH   |
	| ADAMS  | RESEARCH   |
	| FORD   | RESEARCH   |
	| ALLEN  | SALES      |
	| WARD   | SALES      |
	| MARTIN | SALES      |
	| BLAKE  | SALES      |
	| TURNER | SALES      |
	| JAMES  | SALES      |
	+--------+------------+
	
	思考：最终查询的结果条数是14条，但是匹配的过程中，匹配的次数减少了吗？
		还是56次，只不过进行了四选一。次数没有减少。
	
	注意：通过笛卡尔积现象得出，表的连接次数越多效率越低，尽量避免表的
	连接次数。

2.5、内连接之等值连接。

案例：查询每个员工所在部门名称，显示员工名和部门名？
emp e和dept d表进行连接。条件是：e.deptno = d.deptno

SQL92语法：
	select 
		e.ename,d.dname
	from
		emp e, dept d
	where
		e.deptno = d.deptno;
	
	sql92的缺点：结构不清晰，表的连接条件，和后期进一步筛选的条件，都放到了where后面。

SQL99语法：
	select 
		e.ename,d.dname
	from
		emp e
	join
		dept d
	on
		e.deptno = d.deptno;
	

	//inner可以省略（带着inner可读性更好！！！一眼就能看出来是内连接）
	select 
		e.ename,d.dname
	from
		emp e
	inner join
		dept d
	on
		e.deptno = d.deptno; // 条件是等量关系，所以被称为等值连接。


​	
​	sql99优点：表连接的条件是独立的，连接之后，如果还需要进一步筛选，再往后继续添加where
​	
	SQL99语法：
		select 
			...
		from
			a
		join
			b
		on
			a和b的连接条件
		where
			筛选条件

2.6、内连接之非等值连接

案例：找出每个员工的薪资等级，要求显示员工名、薪资、薪资等级？
mysql> select * from emp; e
+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
....

mysql> select * from salgrade; s
+-------+-------+-------+
| GRADE | LOSAL | HISAL |
+-------+-------+-------+
|     1 |   700 |  1200 |
|     2 |  1201 |  1400 |
|     3 |  1401 |  2000 |
|     4 |  2001 |  3000 |
|     5 |  3001 |  9999 |
+-------+-------+-------+

select 
	e.ename, e.sal, s.grade
from
	emp e
join
	salgrade s
on
	e.sal between s.losal and s.hisal; // 条件不是一个等量关系，称为非等值连接。



select 
	e.ename, e.sal, s.grade
from
	emp e
inner join
	salgrade s
on
	e.sal between s.losal and s.hisal;

+--------+---------+-------+
| ename  | sal     | grade |
+--------+---------+-------+
| SMITH  |  800.00 |     1 |
| ALLEN  | 1600.00 |     3 |
| WARD   | 1250.00 |     2 |
| JONES  | 2975.00 |     4 |
| MARTIN | 1250.00 |     2 |
| BLAKE  | 2850.00 |     4 |
| CLARK  | 2450.00 |     4 |
| SCOTT  | 3000.00 |     4 |
| KING   | 5000.00 |     5 |
| TURNER | 1500.00 |     3 |
| ADAMS  | 1100.00 |     1 |
| JAMES  |  950.00 |     1 |
| FORD   | 3000.00 |     4 |
| MILLER | 1300.00 |     2 |
+--------+---------+-------+

2.7、内连接之自连接
案例：查询员工的上级领导，要求显示员工名和对应的领导名？

mysql> select empno,ename,mgr from emp;
+-------+--------+------+ 
| empno | ename  | mgr  |
+-------+--------+------+
|  7369 | SMITH  | 7902 |
|  7499 | ALLEN  | 7698 |
|  7521 | WARD   | 7698 |
|  7566 | JONES  | 7839 |
|  7654 | MARTIN | 7698 |
|  7698 | BLAKE  | 7839 |
|  7782 | CLARK  | 7839 |
|  7788 | SCOTT  | 7566 |
|  7839 | KING   | NULL |
|  7844 | TURNER | 7698 |
|  7876 | ADAMS  | 7788 |
|  7900 | JAMES  | 7698 |
|  7902 | FORD   | 7566 |
|  7934 | MILLER | 7782 |
+-------+--------+------+

技巧：一张表看成两张表。
emp a 员工表
+-------+--------+------+
| empno | ename  | mgr  |
+-------+--------+------+
|  7369 | SMITH  | 7902 |
|  7499 | ALLEN  | 7698 |
|  7521 | WARD   | 7698 |
|  7566 | JONES  | 7839 |
|  7654 | MARTIN | 7698 |
|  7698 | BLAKE  | 7839 |
|  7782 | CLARK  | 7839 |
|  7788 | SCOTT  | 7566 |
|  7839 | KING   | NULL |
|  7844 | TURNER | 7698 |
|  7876 | ADAMS  | 7788 |
|  7900 | JAMES  | 7698 |
|  7902 | FORD   | 7566 |
|  7934 | MILLER | 7782 |
+-------+--------+------+

emp b 领导表
+-------+--------+------+
| empno | ename  | mgr  |
+-------+--------+------+
|  7369 | SMITH  | 7902 |
|  7499 | ALLEN  | 7698 |
|  7521 | WARD   | 7698 |
|  7566 | JONES  | 7839 |
|  7654 | MARTIN | 7698 |
|  7698 | BLAKE  | 7839 |
|  7782 | CLARK  | 7839 |
|  7788 | SCOTT  | 7566 |
|  7839 | KING   | NULL |
|  7844 | TURNER | 7698 |
|  7876 | ADAMS  | 7788 |
|  7900 | JAMES  | 7698 |
|  7902 | FORD   | 7566 |
|  7934 | MILLER | 7782 |
+-------+--------+------+

select 
	a.ename as '员工名', b.ename as '领导名'
from
	emp a
join
	emp b
on
	a.mgr = b.empno; //员工的领导编号 = 领导的员工编号

+--------+--------+
| 员工名 | 领导名|
+--------+--------+
| SMITH  | FORD   |
| ALLEN  | BLAKE  |
| WARD   | BLAKE  |
| JONES  | KING   |
| MARTIN | BLAKE  |
| BLAKE  | KING   |
| CLARK  | KING   |
| SCOTT  | JONES  |
| TURNER | BLAKE  |
| ADAMS  | SCOTT  |
| JAMES  | BLAKE  |
| FORD   | JONES  |
| MILLER | CLARK  |
+--------+--------+
13条记录，没有KING。《内连接》

以上就是内连接中的：自连接，技巧：一张表看做两张表。

2.8、外连接

mysql> select * from emp; e
+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+

mysql> select * from dept; d
+--------+------------+----------+
| DEPTNO | DNAME      | LOC      |
+--------+------------+----------+
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
+--------+------------+----------+

内连接：（A和B连接，AB两张表没有主次关系。平等的。）
select 
	e.ename,d.dname
from
	emp e
join
	dept d
on
	e.deptno = d.deptno; //内连接的特点：完成能够匹配上这个条件的数据查询出来。

+--------+------------+
| ename  | dname      |
+--------+------------+
| CLARK  | ACCOUNTING |
| KING   | ACCOUNTING |
| MILLER | ACCOUNTING |
| SMITH  | RESEARCH   |
| JONES  | RESEARCH   |
| SCOTT  | RESEARCH   |
| ADAMS  | RESEARCH   |
| FORD   | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| TURNER | SALES      |
| JAMES  | SALES      |
+--------+------------+

外连接（右外连接）：
select 
	e.ename,d.dname
from
	emp e 
right join 
	dept d
on
	e.deptno = d.deptno;

// outer是可以省略的，带着可读性强。
select 
	e.ename,d.dname
from
	emp e 
right outer join 
	dept d
on
	e.deptno = d.deptno;

right代表什么：表示将join关键字右边的这张表看成主表，主要是为了将这张表的数据全部查询出来，捎带着关联查询左边的表。
在外连接当中，两张表连接，产生了主次关系。

外连接（左外连接）：
select 
	e.ename,d.dname
from
	dept d 
left join 
	emp e
on
	e.deptno = d.deptno;

// outer是可以省略的，带着可读性强。
select 
	e.ename,d.dname
from
	dept d 
left outer join 
	emp e
on
	e.deptno = d.deptno;

带有right的是右外连接，又叫做右连接。
带有left的是左外连接，又叫做左连接。
任何一个右连接都有左连接的写法。
任何一个左连接都有右连接的写法。

+--------+------------+
| ename  | dname      |
+--------+------------+
| CLARK  | ACCOUNTING |
| KING   | ACCOUNTING |
| MILLER | ACCOUNTING |
| SMITH  | RESEARCH   |
| JONES  | RESEARCH   |
| SCOTT  | RESEARCH   |
| ADAMS  | RESEARCH   |
| FORD   | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| TURNER | SALES      |
| JAMES  | SALES      |
| NULL   | OPERATIONS |
+--------+------------+

思考：外连接的查询结果条数一定是 >= 内连接的查询结果条数？
	正确。


案例：查询每个员工的上级领导，要求显示所有员工的名字和领导名？
	select 
		a.ename as '员工名', b.ename as '领导名'
	from
		emp a
	left join
		emp b
	on
		a.mgr = b.empno; 
	
	+--------+--------+
	| 员工名      | 领导名     |
	+--------+--------+
	| SMITH  | FORD   |
	| ALLEN  | BLAKE  |
	| WARD   | BLAKE  |
	| JONES  | KING   |
	| MARTIN | BLAKE  |
	| BLAKE  | KING   |
	| CLARK  | KING   |
	| SCOTT  | JONES  |
	| KING   | NULL   |
	| TURNER | BLAKE  |
	| ADAMS  | SCOTT  |
	| JAMES  | BLAKE  |
	| FORD   | JONES  |
	| MILLER | CLARK  |
	+--------+--------+


2.9、三张表，四张表怎么连接？
	语法：
		select 
			...
		from
			a
		join
			b
		on
			a和b的连接条件
		join
			c
		on
			a和c的连接条件
		right join
			d
		on
			a和d的连接条件
		
		一条SQL中内连接和外连接可以混合。都可以出现！
	
	案例：找出每个员工的部门名称以及工资等级，
	要求显示员工名、部门名、薪资、薪资等级？
	
	select 
		e.ename,e.sal,d.dname,s.grade
	from
		emp e
	join
		dept d
	on 
		e.deptno = d.deptno
	join
		salgrade s
	on
		e.sal between s.losal and s.hisal;
	
	+--------+---------+------------+-------+
	| ename  | sal     | dname      | grade |
	+--------+---------+------------+-------+
	| SMITH  |  800.00 | RESEARCH   |     1 |
	| ALLEN  | 1600.00 | SALES      |     3 |
	| WARD   | 1250.00 | SALES      |     2 |
	| JONES  | 2975.00 | RESEARCH   |     4 |
	| MARTIN | 1250.00 | SALES      |     2 |
	| BLAKE  | 2850.00 | SALES      |     4 |
	| CLARK  | 2450.00 | ACCOUNTING |     4 |
	| SCOTT  | 3000.00 | RESEARCH   |     4 |
	| KING   | 5000.00 | ACCOUNTING |     5 |
	| TURNER | 1500.00 | SALES      |     3 |
	| ADAMS  | 1100.00 | RESEARCH   |     1 |
	| JAMES  |  950.00 | SALES      |     1 |
	| FORD   | 3000.00 | RESEARCH   |     4 |
	| MILLER | 1300.00 | ACCOUNTING |     2 |
	+--------+---------+------------+-------+
	
	案例：找出每个员工的部门名称以及工资等级，还有上级领导，
	要求显示员工名、领导名、部门名、薪资、薪资等级？
	
	select 
		e.ename,e.sal,d.dname,s.grade,l.ename
	from
		emp e
	join
		dept d
	on 
		e.deptno = d.deptno
	join
		salgrade s
	on
		e.sal between s.losal and s.hisal
	left join
		emp l
	on
		e.mgr = l.empno;
	
	+--------+---------+------------+-------+-------+
	| ename  | sal     | dname      | grade | ename |
	+--------+---------+------------+-------+-------+
	| SMITH  |  800.00 | RESEARCH   |     1 | FORD  |
	| ALLEN  | 1600.00 | SALES      |     3 | BLAKE |
	| WARD   | 1250.00 | SALES      |     2 | BLAKE |
	| JONES  | 2975.00 | RESEARCH   |     4 | KING  |
	| MARTIN | 1250.00 | SALES      |     2 | BLAKE |
	| BLAKE  | 2850.00 | SALES      |     4 | KING  |
	| CLARK  | 2450.00 | ACCOUNTING |     4 | KING  |
	| SCOTT  | 3000.00 | RESEARCH   |     4 | JONES |
	| KING   | 5000.00 | ACCOUNTING |     5 | NULL  |
	| TURNER | 1500.00 | SALES      |     3 | BLAKE |
	| ADAMS  | 1100.00 | RESEARCH   |     1 | SCOTT |
	| JAMES  |  950.00 | SALES      |     1 | BLAKE |
	| FORD   | 3000.00 | RESEARCH   |     4 | JONES |
	| MILLER | 1300.00 | ACCOUNTING |     2 | CLARK |
	+--------+---------+------------+-------+-------+

### 3、子查询？

3.1、什么是子查询？
	select语句中嵌套select语句，被嵌套的select语句称为子查询。

3.2、子查询都可以出现在哪里呢？
	select
		..(select).
	from
		..(select).
	where
		..(select).

3.3、where子句中的子查询

	案例：找出比最低工资高的员工姓名和工资？
		select 
			ename,sal
		from
			emp 
		where
			sal > min(sal);
	
		ERROR 1111 (HY000): Invalid use of group function
		where子句中不能直接使用分组函数。
	
	实现思路：
		第一步：查询最低工资是多少
			select min(sal) from emp;
			+----------+
			| min(sal) |
			+----------+
			|   800.00 |
			+----------+
		第二步：找出>800的
			select ename,sal from emp where sal > 800;
		
		第三步：合并
			select ename,sal from emp where sal > (select min(sal) from emp);
			+--------+---------+
			| ename  | sal     |
			+--------+---------+
			| ALLEN  | 1600.00 |
			| WARD   | 1250.00 |
			| JONES  | 2975.00 |
			| MARTIN | 1250.00 |
			| BLAKE  | 2850.00 |
			| CLARK  | 2450.00 |
			| SCOTT  | 3000.00 |
			| KING   | 5000.00 |
			| TURNER | 1500.00 |
			| ADAMS  | 1100.00 |
			| JAMES  |  950.00 |
			| FORD   | 3000.00 |
			| MILLER | 1300.00 |
			+--------+---------+

3.4、from子句中的子查询
	注意：from后面的子查询，可以将子查询的查询结果当做一张临时表。（技巧）

	案例：找出每个岗位的平均工资的薪资等级。
	
	第一步：找出每个岗位的平均工资（按照岗位分组求平均值）
		select job,avg(sal) from emp group by job;
		+-----------+-------------+
		| job       | avgsal      |
		+-----------+-------------+
		| ANALYST   | 3000.000000 |
		| CLERK     | 1037.500000 |
		| MANAGER   | 2758.333333 |
		| PRESIDENT | 5000.000000 |
		| SALESMAN  | 1400.000000 |
		+-----------+-------------+t表
	
	第二步：克服心理障碍，把以上的查询结果就当做一张真实存在的表t。
	mysql> select * from salgrade; s表
	+-------+-------+-------+
	| GRADE | LOSAL | HISAL |
	+-------+-------+-------+
	|     1 |   700 |  1200 |
	|     2 |  1201 |  1400 |
	|     3 |  1401 |  2000 |
	|     4 |  2001 |  3000 |
	|     5 |  3001 |  9999 |
	+-------+-------+-------+
	t表和s表进行表连接，条件：t表avg(sal) between s.losal and s.hisal;
		
		select 
			t.*, s.grade
		from
			(select job,avg(sal) as avgsal from emp group by job) t
		join
			salgrade s
		on
			t.avgsal between s.losal and s.hisal;
		
		+-----------+-------------+-------+
		| job       | avgsal      | grade |
		+-----------+-------------+-------+
		| CLERK     | 1037.500000 |     1 |
		| SALESMAN  | 1400.000000 |     2 |
		| ANALYST   | 3000.000000 |     4 |
		| MANAGER   | 2758.333333 |     4 |
		| PRESIDENT | 5000.000000 |     5 |
		+-----------+-------------+-------+

3.5、select后面出现的子查询（这个内容不需要掌握，了解即可！！！）

案例：找出每个员工的部门名称，要求显示员工名，部门名？
	select 
		e.ename,e.deptno,(select d.dname from dept d where e.deptno = d.deptno) as dname 
	from 
		emp e;


	+--------+--------+------------+
	| ename  | deptno | dname      |
	+--------+--------+------------+
	| SMITH  |     20 | RESEARCH   |
	| ALLEN  |     30 | SALES      |
	| WARD   |     30 | SALES      |
	| JONES  |     20 | RESEARCH   |
	| MARTIN |     30 | SALES      |
	| BLAKE  |     30 | SALES      |
	| CLARK  |     10 | ACCOUNTING |
	| SCOTT  |     20 | RESEARCH   |
	| KING   |     10 | ACCOUNTING |
	| TURNER |     30 | SALES      |
	| ADAMS  |     20 | RESEARCH   |
	| JAMES  |     30 | SALES      |
	| FORD   |     20 | RESEARCH   |
	| MILLER |     10 | ACCOUNTING |
	+--------+--------+------------+
	
	//错误：ERROR 1242 (21000): Subquery returns more than 1 row
	select 
		e.ename,e.deptno,(select dname from dept) as dname
	from
		emp e;
	
	注意：对于select后面的子查询来说，这个子查询只能一次返回1条结果，
	多于1条，就报错了。！

4、union合并查询结果集

案例：查询工作岗位是MANAGER和SALESMAN的员工？
	select ename,job from emp where job = 'MANAGER' or job = 'SALESMAN';
	select ename,job from emp where job in('MANAGER','SALESMAN');
	+--------+----------+
	| ename  | job      |
	+--------+----------+
	| ALLEN  | SALESMAN |
	| WARD   | SALESMAN |
	| JONES  | MANAGER  |
	| MARTIN | SALESMAN |
	| BLAKE  | MANAGER  |
	| CLARK  | MANAGER  |
	| TURNER | SALESMAN |
	+--------+----------+
	
	select ename,job from emp where job = 'MANAGER'
	union
	select ename,job from emp where job = 'SALESMAN';
	
	+--------+----------+
	| ename  | job      |
	+--------+----------+
	| JONES  | MANAGER  |
	| BLAKE  | MANAGER  |
	| CLARK  | MANAGER  |
	| ALLEN  | SALESMAN |
	| WARD   | SALESMAN |
	| MARTIN | SALESMAN |
	| TURNER | SALESMAN |
	+--------+----------+
	
	union的效率要高一些。对于表连接来说，每连接一次新表，
	则匹配的次数满足笛卡尔积，成倍的翻。。。
	但是union可以减少匹配的次数。在减少匹配次数的情况下，
	还可以完成两个结果集的拼接。
	
	a 连接 b 连接 c
	a 10条记录
	b 10条记录
	c 10条记录
	匹配次数是：1000
	
	a 连接 b一个结果：10 * 10 --> 100次
	a 连接 c一个结果：10 * 10 --> 100次
	使用union的话是：100次 + 100次 = 200次。（union把乘法变成了加法运算）

union在使用的时候有注意事项吗？

	//错误的：union在进行结果集合并的时候，要求两个结果集的列数相同。
	select ename,job from emp where job = 'MANAGER'
	union
	select ename from emp where job = 'SALESMAN';
	
	// MYSQL可以，oracle语法严格 ，不可以，报错。要求：结果集合并时列和列的数据类型也要一致。
	select ename,job from emp where job = 'MANAGER'
	union
	select ename,sal from emp where job = 'SALESMAN';
	+--------+---------+
	| ename  | job     |
	+--------+---------+
	| JONES  | MANAGER |
	| BLAKE  | MANAGER |
	| CLARK  | MANAGER |
	| ALLEN  | 1600    |
	| WARD   | 1250    |
	| MARTIN | 1250    |
	| TURNER | 1500    |
	+--------+---------+

5、limit（非常重要）
	
5.1、limit作用：将查询结果集的一部分取出来。通常使用在分页查询当中。
百度默认：一页显示10条记录。
分页的作用是为了提高用户的体验，因为一次全部都查出来，用户体验差。
可以一页一页翻页看。

5.2、limit怎么用呢？

	完整用法：limit startIndex, length
		startIndex是起始下标，length是长度。
		起始下标从0开始。
	
	缺省用法：limit 5; 这是取前5.
	
	按照薪资降序，取出排名在前5名的员工？
	select 
		ename,sal
	from
		emp
	order by 
		sal desc
	limit 5; //取前5
	
	select 
		ename,sal
	from
		emp
	order by 
		sal desc
	limit 0,5;
	
	+-------+---------+
	| ename | sal     |
	+-------+---------+
	| KING  | 5000.00 |
	| SCOTT | 3000.00 |
	| FORD  | 3000.00 |
	| JONES | 2975.00 |
	| BLAKE | 2850.00 |
	+-------+---------+

5.3、注意：mysql当中limit在order by之后执行！！！！！！

5.4、取出工资排名在[3-5]名的员工？
	select 
		ename,sal
	from
		emp
	order by
		sal desc
	limit
		2, 3;
	
	2表示起始位置从下标2开始，就是第三条记录。
	3表示长度。
	
	+-------+---------+
	| ename | sal     |
	+-------+---------+
	| FORD  | 3000.00 |
	| JONES | 2975.00 |
	| BLAKE | 2850.00 |
	+-------+---------+

5.5、取出工资排名在[5-9]名的员工？
	select 
		ename,sal
	from
		emp
	order by 
		sal desc
	limit
		4, 5;

	+--------+---------+
	| ename  | sal     |
	+--------+---------+
	| BLAKE  | 2850.00 |
	| CLARK  | 2450.00 |
	| ALLEN  | 1600.00 |
	| TURNER | 1500.00 |
	| MILLER | 1300.00 |
	+--------+---------+

5.6、分页

每页显示3条记录
	第1页：limit 0,3		[0 1 2]
	第2页：limit 3,3		[3 4 5]
	第3页：limit 6,3		[6 7 8]
	第4页：limit 9,3		[9 10 11]

每页显示pageSize条记录
	第pageNo页：limit (pageNo - 1) * pageSize  , pageSize

	public static void main(String[] args){
		// 用户提交过来一个页码，以及每页显示的记录条数
		int pageNo = 5; //第5页
		int pageSize = 10; //每页显示10条
	
		int startIndex = (pageNo - 1) * pageSize;
		String sql = "select ...limit " + startIndex + ", " + pageSize;
	}

记公式：
	limit (pageNo-1)*pageSize , pageSize

6、关于DQL语句的大总结：
	select 
		...
	from
		...
	where
		...
	group by
		...
	having
		...
	order by
		...
	limit
		...
	
	执行顺序？
		1.from
		2.where
		3.group by
		4.having
		5.select
		6.order by
		7.limit..

7、表的创建（建表）

7.1、建表的语法格式：(建表属于DDL语句，DDL包括：create drop alter)

	create table 表名(字段名1 数据类型, 字段名2 数据类型, 字段名3 数据类型);
	
	create table 表名(
		字段名1 数据类型, 
		字段名2 数据类型, 
		字段名3 数据类型
	);
	
	表名：建议以t_ 或者 tbl_开始，可读性强。见名知意。
	字段名：见名知意。
	表名和字段名都属于标识符。

7.2、关于mysql中的数据类型？

	很多数据类型，我们只需要掌握一些常见的数据类型即可。
	
		varchar(最长255)
			可变长度的字符串
			比较智能，节省空间。
			会根据实际的数据长度动态分配空间。
	
			优点：节省空间
			缺点：需要动态分配空间，速度慢。
	
		char(最长255)
			定长字符串
			不管实际的数据长度是多少。
			分配固定长度的空间去存储数据。
			使用不恰当的时候，可能会导致空间的浪费。
	
			优点：不需要动态分配空间，速度快。
			缺点：使用不当可能会导致空间的浪费。
	
			varchar和char我们应该怎么选择？
				性别字段你选什么？因为性别是固定长度的字符串，所以选择char。
				姓名字段你选什么？每一个人的名字长度不同，所以选择varchar。
	
		int(最长11)
			数字中的整数型。等同于java的int。
	
		bigint
			数字中的长整型。等同于java中的long。
	
		float	
			单精度浮点型数据
	
		double
			双精度浮点型数据
	
		date
			短日期类型
	
		datetime
			长日期类型
	
		clob
			字符大对象
			最多可以存储4G的字符串。
			比如：存储一篇文章，存储一个说明。
			超过255个字符的都要采用CLOB字符大对象来存储。
			Character Large OBject:CLOB


		blob
			二进制大对象
			Binary Large OBject
			专门用来存储图片、声音、视频等流媒体数据。
			往BLOB类型的字段上插入数据的时候，例如插入一个图片、视频等，
			你需要使用IO流才行。
	
	t_movie 电影表（专门存储电影信息的）
	
	编号			名字				故事情节					上映日期				时长				海报					类型
	no(bigint)	name(varchar)	history(clob)		playtime(date)		time(double)	image(blob)			type(char)
	------------------------------------------------------------------------------------------------------------------
	10000			哪吒				...........			2019-10-11			2.5				....					'1'
	10001			林正英之娘娘   ...........			2019-11-11			1.5				....					'2'
	....

7.3、创建一个学生表？
	学号、姓名、年龄、性别、邮箱地址
	create table t_student(
		no int,
		name varchar(32),
		sex char(1),
		age int(3),
		email varchar(255)
	);

	删除表：
		drop table t_student; // 当这张表不存在的时候会报错！
	
		// 如果这张表存在的话，删除
		drop table if exists t_student;

7.4、插入数据insert （DML）
	
	语法格式：
		insert into 表名(字段名1,字段名2,字段名3...) values(值1,值2,值3);
	
		注意：字段名和值要一一对应。什么是一一对应？
			数量要对应。数据类型要对应。
	
	insert into t_student(no,name,sex,age,email) values(1,'zhangsan','m',20,'zhangsan@123.com');
	insert into t_student(email,name,sex,age,no) values('lisi@123.com','lisi','f',20,2);
	
	insert into t_student(no) values(3);
	
	+------+----------+------+------+------------------+
	| no   | name     | sex  | age  | email            |
	+------+----------+------+------+------------------+
	|    1 | zhangsan | m    |   20 | zhangsan@123.com |
	|    2 | lisi     | f    |   20 | lisi@123.com     |
	|    3 | NULL     | NULL | NULL | NULL             |
	+------+----------+------+------+------------------+
	insert into t_student(name) values('wangwu');
	+------+----------+------+------+------------------+
	| no   | name     | sex  | age  | email            |
	+------+----------+------+------+------------------+
	|    1 | zhangsan | m    |   20 | zhangsan@123.com |
	|    2 | lisi     | f    |   20 | lisi@123.com     |
	|    3 | NULL     | NULL | NULL | NULL             |
	| NULL | wangwu   | NULL | NULL | NULL             |
	+------+----------+------+------+------------------+
	注意：insert语句但凡是执行成功了，那么必然会多一条记录。
	没有给其它字段指定值的话，默认值是NULL。
	
	drop table if exists t_student;
	create table t_student(
		no int,
		name varchar(32),
		sex char(1) default 'm',
		age int(3),
		email varchar(255)
	);
	
	+-------+--------------+------+-----+---------+-------+
	| Field | Type         | Null | Key | Default | Extra |
	+-------+--------------+------+-----+---------+-------+
	| no    | int(11)      | YES  |     | NULL    |       |
	| name  | varchar(32)  | YES  |     | NULL    |       |
	| sex   | char(1)      | YES  |     | m       |       |
	| age   | int(3)       | YES  |     | NULL    |       |
	| email | varchar(255) | YES  |     | NULL    |       |
	+-------+--------------+------+-----+---------+-------+
	insert into t_student(no) values(1);
	mysql> select * from t_student;
	+------+------+------+------+-------+
	| no   | name | sex  | age  | email |
	+------+------+------+------+-------+
	|    1 | NULL | m    | NULL | NULL  |
	+------+------+------+------+-------+
	
	insert语句中的“字段名”可以省略吗？可以
		insert into t_student values(2); //错误的
	
		// 注意：前面的字段名省略的话，等于都写上了！所以值也要都写上！
		insert into t_student values(2, 'lisi', 'f', 20, 'lisi@123.com');
		+------+------+------+------+--------------+
		| no   | name | sex  | age  | email        |
		+------+------+------+------+--------------+
		|    1 | NULL | m    | NULL | NULL         |
		|    2 | lisi | f    |   20 | lisi@123.com |
		+------+------+------+------+--------------+

7.5、insert插入日期

	数字格式化：format
		select ename,sal from emp;
		+--------+---------+
		| ename  | sal     |
		+--------+---------+
		| SMITH  |  800.00 |
		| ALLEN  | 1600.00 |
		| WARD   | 1250.00 |
		| JONES  | 2975.00 |
		| MARTIN | 1250.00 |
		| BLAKE  | 2850.00 |
		| CLARK  | 2450.00 |
		| SCOTT  | 3000.00 |
		| KING   | 5000.00 |
		| TURNER | 1500.00 |
		| ADAMS  | 1100.00 |
		| JAMES  |  950.00 |
		| FORD   | 3000.00 |
		| MILLER | 1300.00 |
		+--------+---------+
	
		格式化数字：format(数字, '格式')
			select ename,format(sal, '$999,999') as sal from emp;
			+--------+-------+
			| ename  | sal   |
			+--------+-------+
			| SMITH  | 800   |
			| ALLEN  | 1,600 |
			| WARD   | 1,250 |
			| JONES  | 2,975 |
			| MARTIN | 1,250 |
			| BLAKE  | 2,850 |
			| CLARK  | 2,450 |
			| SCOTT  | 3,000 |
			| KING   | 5,000 |
			| TURNER | 1,500 |
			| ADAMS  | 1,100 |
			| JAMES  | 950   |
			| FORD   | 3,000 |
			| MILLER | 1,300 |
			+--------+-------+
	
	str_to_date：将字符串varchar类型转换成date类型
	date_format：将date类型转换成具有一定格式的varchar字符串类型。
	
	drop table if exists t_user;
	create table t_user(
		id int,
		name varchar(32),
		birth date // 生日也可以使用date日期类型
	);
	
	create table t_user(
		id int,
		name varchar(32),
		birth char(10) // 生日可以使用字符串，没问题。
	);
	
	生日：1990-10-11 （10个字符）
	
	注意：数据库中的有一条命名规范：
		所有的标识符都是全部小写，单词和单词之间使用下划线进行衔接。
	
	mysql> desc t_user;
	+-------+-------------+------+-----+---------+-------+
	| Field | Type        | Null | Key | Default | Extra |
	+-------+-------------+------+-----+---------+-------+
	| id    | int(11)     | YES  |     | NULL    |       |
	| name  | varchar(32) | YES  |     | NULL    |       |
	| birth | date        | YES  |     | NULL    |       |
	+-------+-------------+------+-----+---------+-------+
	
	插入数据？
		insert into t_user(id,name,birth) values(1, 'zhangsan', '01-10-1990'); // 1990年10月1日
		出问题了：原因是类型不匹配。数据库birth是date类型，这里给了一个字符串varchar。
	
		怎么办？可以使用str_to_date函数进行类型转换。
		str_to_date函数可以将字符串转换成日期类型date？
		语法格式：
			str_to_date('字符串日期', '日期格式')
	
		mysql的日期格式：
			%Y	年
			%m 月
			%d 日
			%h	时
			%i	分
			%s	秒
		
		insert into t_user(id,name,birth) values(1, 'zhangsan', str_to_date('01-10-1990','%d-%m-%Y'));
	
		str_to_date函数可以把字符串varchar转换成日期date类型数据，
		通常使用在插入insert方面，因为插入的时候需要一个日期类型的数据，
		需要通过该函数将字符串转换成date。
	
	好消息？
		如果你提供的日期字符串是这个格式，str_to_date函数就不需要了！！！
			%Y-%m-%d
		insert into t_user(id,name,birth) values(2, 'lisi', '1990-10-01');
	
	查询的时候可以以某个特定的日期格式展示吗？
		date_format
		这个函数可以将日期类型转换成特定格式的字符串。
	
		select id,name,date_format(birth, '%m/%d/%Y') as birth from t_user;
		+------+----------+------------+
		| id   | name     | birth      |
		+------+----------+------------+
		|    1 | zhangsan | 10/01/1990 |
		|    2 | lisi     | 10/01/1990 |
		+------+----------+------------+
	
		date_format函数怎么用？
			date_format(日期类型数据, '日期格式')
			这个函数通常使用在查询日期方面。设置展示的日期格式。
		
		mysql> select id,name,birth from t_user;
		+------+----------+------------+
		| id   | name     | birth      |
		+------+----------+------------+
		|    1 | zhangsan | 1990-10-01 |
		|    2 | lisi     | 1990-10-01 |
		+------+----------+------------+
		以上的SQL语句实际上是进行了默认的日期格式化，
		自动将数据库中的date类型转换成varchar类型。
		并且采用的格式是mysql默认的日期格式：'%Y-%m-%d'
	
		select id,name,date_format(birth,'%Y/%m/%d') as birth from t_user;
		
		java中的日期格式？
			yyyy-MM-dd HH:mm:ss SSS

7.6、date和datetime两个类型的区别？
	date是短日期：只包括年月日信息。
	datetime是长日期：包括年月日时分秒信息。

	drop table if exists t_user;
	create table t_user(
		id int,
		name varchar(32),
		birth date,
		create_time datetime
	);


	id是整数
	name是字符串
	birth是短日期
	create_time是这条记录的创建时间：长日期类型
	
	mysql短日期默认格式：%Y-%m-%d
	mysql长日期默认格式：%Y-%m-%d %h:%i:%s
	
	insert into t_user(id,name,birth,create_time) values(1,'zhangsan','1990-10-01','2020-03-18 15:49:50');
	
	在mysql当中怎么获取系统当前时间？
		now() 函数，并且获取的时间带有：时分秒信息！！！！是datetime类型的。
	
		insert into t_user(id,name,birth,create_time) values(2,'lisi','1991-10-01',now());

7.7、修改update（DML）

语法格式：
	update 表名 set 字段名1=值1,字段名2=值2,字段名3=值3... where 条件;

	注意：没有条件限制会导致所有数据全部更新。
	
	update t_user set name = 'jack', birth = '2000-10-11' where id = 2;
	+------+----------+------------+---------------------+
	| id   | name     | birth      | create_time         |
	+------+----------+------------+---------------------+
	|    1 | zhangsan | 1990-10-01 | 2020-03-18 15:49:50 |
	|    2 | jack     | 2000-10-11 | 2020-03-18 15:51:23 |
	+------+----------+------------+---------------------+
	
	update t_user set name = 'jack', birth = '2000-10-11', create_time = now() where id = 2;
	
	更新所有？
		update t_user set name = 'abc';

7.8、删除数据 delete （DML）
	语法格式？
		delete from 表名 where 条件;

	注意：没有条件，整张表的数据会全部删除！
	
	delete from t_user where id = 2;
	
	insert into t_user(id) values(2);
	
	delete from t_user; // 删除所有！