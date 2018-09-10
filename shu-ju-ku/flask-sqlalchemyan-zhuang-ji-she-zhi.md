# Flask-SQLAlchemy安装及设置

  * SQLALchemy 实际上是对数据库的抽象，让开发者不用直接和 SQL 语句打交道，而是通过 Python 对象来操作数据库，在舍弃一些性能开销的同时，换来的是开发效率的较大提升
  * SQLAlchemy是一个关系型数据库框架，它提供了高层的 ORM 和底层的原生数据库的操作。flask-sqlalchemy 是一个简化了 SQLAlchemy 操作的flask扩展。
  * 文档地址：http://docs.jinkan.org/docs/flask-sqlalchemy

## 安装

  * 安装 flask-sqlalchemy

{%ace edit=true, lang='python'%}

    pip install flask-sqlalchemy
    
{%endace%}

  * 如果连接的是 mysql 数据库，需要安装 mysqldb

{%ace edit=true, lang='python'%}

    pip install flask-mysqldb
    
{%endace%}

## 数据库连接设置

  * 在 Flask-SQLAlchemy 中，数据库使用URL指定，而且程序使用的数据库必须保存到Flask配置对象的 **SQLALCHEMY\_DATABASE\_URI** 键中

{%ace edit=true, lang='python'%}

    app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:mysql@127.0.0.1:3306/test'
    
{%endace%}

  * 其他设置：

{%ace edit=true, lang='python'%}

    # 动态追踪修改设置，如未设置只会提示警告
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True
    #查询时会显示原始SQL语句
    app.config['SQLALCHEMY_ECHO'] = True
    
{%endace%}

  * 配置完成需要去 MySQL 中创建项目所使用的数据库

{%ace edit=true, lang='python'%}

    $ mysql -uroot -pmysql
    $ create database test charset utf8;
    
{%endace%}

  * 其他配置

  
<table>  
<tr>  
<th>

名字

</th>  
<th>

备注

</th> </tr>  
<tr>  
<td>

SQLALCHEMY\_DATABASE\_URI

</td>  
<td>

用于连接的数据库 URI 。例如:sqlite:////tmp/test.dbmysql://username:password@server/db

</td> </tr>  
<tr>  
<td>

SQLALCHEMY\_BINDS

</td>  
<td>

一个映射 binds 到连接 URI 的字典。更多 binds 的信息见 _用 Binds  操作多个数据库_。

</td> </tr>  
<tr>  
<td>

SQLALCHEMY\_ECHO

</td>  
<td>

如果设置为Ture， SQLAlchemy 会记录所有 发给 stderr 的语句，这对调试有用。\(打印sql语句\)

</td> </tr>  
<tr>  
<td>

SQLALCHEMY\_RECORD\_QUERIES

</td>  
<td>

可以用于显式地禁用或启用查询记录。查询记录 在调试或测试模式自动启用。更多信息见get\_debug\_queries\(\)。

</td> </tr>  
<tr>  
<td>

SQLALCHEMY\_NATIVE\_UNICODE

</td>  
<td>

可以用于显式禁用原生 unicode 支持。当使用 不合适的指定无编码的数据库默认值时，这对于 一些数据库适配器是必须的（比如 Ubuntu 上 某些版本的
PostgreSQL ）。

</td> </tr>  
<tr>  
<td>

SQLALCHEMY\_POOL\_SIZE

</td>  
<td>

数据库连接池的大小。默认是引擎默认值（通常 是 5 ）

</td> </tr>  
<tr>  
<td>

SQLALCHEMY\_POOL\_TIMEOUT

</td>  
<td>

设定连接池的连接超时时间。默认是 10 。

</td> </tr>  
<tr>  
<td>

SQLALCHEMY\_POOL\_RECYCLE

</td>  
<td>

多少秒后自动回收连接。这对 MySQL 是必要的， 它默认移除闲置多于 8 小时的连接。注意如果 使用了 MySQL ， Flask-SQLALchemy
自动设定 这个值为 2 小时。

</td> </tr> </table>

### 连接其他数据库

完整连接 URI 列表请跳转到 SQLAlchemy 下面的文档 \(Supported Databases\) 。这里给出一些 常见的连接字符串。

  * Postgres:

{%ace edit=true, lang='python'%}

    postgresql://scott:tiger@localhost/mydatabase
    
{%endace%}

  * MySQL:

{%ace edit=true, lang='python'%}

    mysql://scott:tiger@localhost/mydatabase
    
{%endace%}

  * Oracle:

{%ace edit=true, lang='python'%}

    - oracle://scott:tiger@127.0.0.1:1521/sidname
    
{%endace%}

  * SQLite （注意开头的四个斜线）:

{%ace edit=true, lang='python'%}

    sqlite:////absolute/path/to/foo.db
    
{%endace%}

## 常用的SQLAlchemy字段类型  
  
<table>  
<tr>  
<th>

类型名

</th>  
<th>

python中类型

</th>  
<th>

说明

</th> </tr>  
<tr>  
<td>

Integer

</td>  
<td>

int

</td>  
<td>

普通整数，一般是32位

</td> </tr>  
<tr>  
<td>

SmallInteger

</td>  
<td>

int

</td>  
<td>

取值范围小的整数，一般是16位

</td> </tr>  
<tr>  
<td>

BigInteger

</td>  
<td>

int或long

</td>  
<td>

不限制精度的整数

</td> </tr>  
<tr>  
<td>

Float

</td>  
<td>

float

</td>  
<td>

浮点数

</td> </tr>  
<tr>  
<td>

Numeric

</td>  
<td>

decimal.Decimal

</td>  
<td>

普通整数，一般是32位

</td> </tr>  
<tr>  
<td>

String

</td>  
<td>

str

</td>  
<td>

变长字符串

</td> </tr>  
<tr>  
<td>

Text

</td>  
<td>

str

</td>  
<td>

变长字符串，对较长或不限长度的字符串做了优化

</td> </tr>  
<tr>  
<td>

Unicode

</td>  
<td>

unicode

</td>  
<td>

变长Unicode字符串

</td> </tr>  
<tr>  
<td>

UnicodeText

</td>  
<td>

unicode

</td>  
<td>

变长Unicode字符串，对较长或不限长度的字符串做了优化

</td> </tr>  
<tr>  
<td>

Boolean

</td>  
<td>

bool

</td>  
<td>

布尔值

</td> </tr>  
<tr>  
<td>

Date

</td>  
<td>

datetime.date

</td>  
<td>

时间

</td> </tr>  
<tr>  
<td>

Time

</td>  
<td>

datetime.datetime

</td>  
<td>

日期和时间

</td> </tr>  
<tr>  
<td>

LargeBinary

</td>  
<td>

str

</td>  
<td>

二进制文件

</td> </tr> </table>

## 常用的SQLAlchemy列选项  
  
<table>  
<tr>  
<th>

选项名

</th>  
<th>

说明

</th> </tr>  
<tr>  
<td>

primary\_key

</td>  
<td>

如果为True，代表表的主键

</td> </tr>  
<tr>  
<td>

unique

</td>  
<td>

如果为True，代表这列不允许出现重复的值

</td> </tr>  
<tr>  
<td>

index

</td>  
<td>

如果为True，为这列创建索引，提高查询效率

</td> </tr>  
<tr>  
<td>

nullable

</td>  
<td>

如果为True，允许有空值，如果为False，不允许有空值

</td> </tr>  
<tr>  
<td>

default

</td>  
<td>

为这列定义默认值

</td> </tr> </table>

## 常用的SQLAlchemy关系选项  
  
<table>  
<tr>  
<th>

选项名

</th>  
<th>

说明

</th> </tr>  
<tr>  
<td>

backref

</td>  
<td>

在关系的另一模型中添加反向引用

</td> </tr>  
<tr>  
<td>

primary join

</td>  
<td>

明确指定两个模型之间使用的联结条件

</td> </tr>  
<tr>  
<td>

uselist

</td>  
<td>

如果为False，不使用列表，而使用标量值

</td> </tr>  
<tr>  
<td>

order\_by

</td>  
<td>

指定关系中记录的排序方式

</td> </tr>  
<tr>  
<td>

secondary

</td>  
<td>

指定多对多关系中关系表的名字

</td> </tr>  
<tr>  
<td>

secondary join

</td>  
<td>

在SQLAlchemy中无法自行决定时，指定多对多关系中的二级联结条件

</td> </tr> </table>

____

