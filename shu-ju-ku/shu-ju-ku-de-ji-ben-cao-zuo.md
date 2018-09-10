# 数据库基本操作

  * 在Flask-SQLAlchemy中，插入、修改、删除操作，均由数据库会话管理。

    * 会话用 db.session 表示。在准备把数据写入数据库前，要先将数据添加到会话中然后调用 commit\(\) 方法提交会话。
  * 在 Flask-SQLAlchemy 中，查询操作是通过 query 对象操作数据。

    * 最基本的查询是返回表中所有数据，可以通过过滤器进行更精确的数据库查询。

### 在视图函数中定义模型类

{%ace edit=true, lang='python'%}

    from flask import Flask
    from flask_sqlalchemy import SQLAlchemy
    
    
    app = Flask(__name__)
    
    #设置连接数据库的URL
    app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:mysql@127.0.0.1:3306/test'
    
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True
    #查询时会显示原始SQL语句
    app.config['SQLALCHEMY_ECHO'] = True
    db = SQLAlchemy(app)
    
    class Role(db.Model):
        # 定义表名
        __tablename__ = 'roles'
        # 定义列对象
        id = db.Column(db.Integer, primary_key=True)
        name = db.Column(db.String(64), unique=True)
        us = db.relationship('User', backref='role')
    
        #repr()方法显示一个可读字符串
        def __repr__(self):
            return 'Role:%s'% self.name
    
    class User(db.Model):
        __tablename__ = 'users'
        id = db.Column(db.Integer, primary_key=True)
        name = db.Column(db.String(64), unique=True, index=True)
        email = db.Column(db.String(64),unique=True)
        password = db.Column(db.String(64))
        role_id = db.Column(db.Integer, db.ForeignKey('roles.id'))
    
        def __repr__(self):
            return 'User:%s'%self.name
    if __name__ == '__main__':
        app.run(debug=True)
    
{%endace%}

### 模型之前的关联

#### 一对多

{%ace edit=true, lang='python'%}

    class Role(db.Model):
        ...
        #关键代码
        us = db.relationship('User', backref='role', lazy='dynamic')
        ...
    
    class User(db.Model):
        ...
        role_id = db.Column(db.Integer, db.ForeignKey('roles.id'))
    
{%endace%}

  * 其中realtionship描述了Role和User的关系。在此文中，第一个参数为对应参照的类"User"
  * 第二个参数backref为类User申明新属性的方法
  * 第三个参数lazy决定了什么时候SQLALchemy从数据库中加载数据
    * 如果设置为子查询方式\(subquery\)，则会在加载完Role对象后，就立即加载与其关联的对象，这样会让总查询数量减少，但如果返回的条目数量很多，就会比较慢
      * 设置为 subquery 的话，role.users 返回所有数据列表
    * 另外,也可以设置为动态方式\(dynamic\)，这样关联对象会在被使用的时候再进行加载，并且在返回前进行过滤，如果返回的对象数很多，或者未来会变得很多，那最好采用这种方式
      * 设置为 dynamic 的话，role.users 返回查询对象，并没有做真正的查询，可以利用查询对象做其他逻辑，比如：先排序再返回结果

#### 多对多

{%ace edit=true, lang='python'%}

    registrations = db.Table('registrations',  
        db.Column('student_id', db.Integer, db.ForeignKey('students.id')),  
        db.Column('course_id', db.Integer, db.ForeignKey('courses.id'))  
    )  
    class Course(db.Model):
        ...
    class Student(db.Model):
        ...
        courses = db.relationship('Course',secondary=registrations,  
                                        backref='students',  
                                        lazy='dynamic')
    
{%endace%}

### 常用的SQLAlchemy查询过滤器  
  
<table>  
<tr>  
<th>

过滤器

</th>  
<th>

说明

</th> </tr>  
<tr>  
<td>

filter\(\)

</td>  
<td>

把过滤器添加到原查询上，返回一个新查询

</td> </tr>  
<tr>  
<td>

filter\_by\(\)

</td>  
<td>

把等值过滤器添加到原查询上，返回一个新查询

</td> </tr>  
<tr>  
<td>

limit

</td>  
<td>

使用指定的值限定原查询返回的结果

</td> </tr>  
<tr>  
<td>

offset\(\)

</td>  
<td>

偏移原查询返回的结果，返回一个新查询

</td> </tr>  
<tr>  
<td>

order\_by\(\)

</td>  
<td>

根据指定条件对原查询结果进行排序，返回一个新查询

</td> </tr>  
<tr>  
<td>

group\_by\(\)

</td>  
<td>

根据指定条件对原查询结果进行分组，返回一个新查询

</td> </tr> </table>

### 常用的SQLAlchemy查询执行器  
  
<table>  
<tr>  
<th>

方法

</th>  
<th>

说明

</th> </tr>  
<tr>  
<td>

all\(\)

</td>  
<td>

以列表形式返回查询的所有结果

</td> </tr>  
<tr>  
<td>

first\(\)

</td>  
<td>

返回查询的第一个结果，如果未查到，返回None

</td> </tr>  
<tr>  
<td>

first\_or\_404\(\)

</td>  
<td>

返回查询的第一个结果，如果未查到，返回404

</td> </tr>  
<tr>  
<td>

get\(\)

</td>  
<td>

返回指定主键对应的行，如不存在，返回None

</td> </tr>  
<tr>  
<td>

get\_or\_404\(\)

</td>  
<td>

返回指定主键对应的行，如不存在，返回404

</td> </tr>  
<tr>  
<td>

count\(\)

</td>  
<td>

返回查询结果的数量

</td> </tr>  
<tr>  
<td>

paginate\(\)

</td>  
<td>

返回一个Paginate对象，它包含指定范围内的结果

</td> </tr> </table>

### 创建表：

{%ace edit=true, lang='python'%}

    db.create_all()
    
{%endace%}

### 删除表

{%ace edit=true, lang='python'%}

    db.drop_all()
    
{%endace%}

### 插入一条数据

{%ace edit=true, lang='python'%}

    ro1 = Role(name='admin')
    db.session.add(ro1)
    db.session.commit()
    #再次插入一条数据
    ro2 = Role(name='user')
    db.session.add(ro2)
    db.session.commit()
    
{%endace%}

### 一次插入多条数据

{%ace edit=true, lang='python'%}

    us1 = User(name='wang',email='wang@163.com',password='123456',role_id=ro1.id)
    us2 = User(name='zhang',email='zhang@189.com',password='201512',role_id=ro2.id)
    us3 = User(name='chen',email='chen@126.com',password='987654',role_id=ro2.id)
    us4 = User(name='zhou',email='zhou@163.com',password='456789',role_id=ro1.id)
    us5 = User(name='tang',email='tang@itheima.com',password='158104',role_id=ro2.id)
    us6 = User(name='wu',email='wu@gmail.com',password='5623514',role_id=ro2.id)
    us7 = User(name='qian',email='qian@gmail.com',password='1543567',role_id=ro1.id)
    us8 = User(name='liu',email='liu@itheima.com',password='867322',role_id=ro1.id)
    us9 = User(name='li',email='li@163.com',password='4526342',role_id=ro2.id)
    us10 = User(name='sun',email='sun@163.com',password='235523',role_id=ro2.id)
    db.session.add_all([us1,us2,us3,us4,us5,us6,us7,us8,us9,us10])
    db.session.commit()
    
    
    """
    查询所有用户数据
    查询有多少个用户
    查询第1个用户
    查询id为4的用户[3种方式]
    查询名字结尾字符为g的所有数据[开始/包含]
    查询名字不等于wang的所有数据[2种方式]
    查询名字和邮箱都以 li 开头的所有数据[2种方式]
    查询password是 `123456` 或者 `email` 以 `itheima.com` 结尾的所有数据
    查询id为 [1, 3, 5, 7, 9] 的用户列表
    查询name为liu的角色数据
    查询所有用户数据，并以邮箱排序
    每页3个，查询第2页的数据
    """
    
{%endace%}

### 查询:filter\_by精确查询

返回名字等于wang的所有人

{%ace edit=true, lang='python'%}

    User.query.filter_by(name='wang').all()
    
{%endace%}

![过滤名字](../assets/filter_by_all.png)

### first\(\)返回查询到的第一个对象

{%ace edit=true, lang='python'%}

    User.query.first()
    
{%endace%}

### all\(\)返回查询到的所有对象

{%ace edit=true, lang='python'%}

    User.query.all()
    
{%endace%}

![查询所有对象](../assets/query_all.png)

### filter模糊查询，返回名字结尾字符为g的所有数据。

{%ace edit=true, lang='python'%}

    User.query.filter(User.name.endswith('g')).all()
    
{%endace%}

![模糊查询](../assets/like_query.png)

### get\(\):参数为主键，如果主键不存在没有返回内容

{%ace edit=true, lang='python'%}

    User.query.get()
    
{%endace%}

### 逻辑非，返回名字不等于wang的所有数据

{%ace edit=true, lang='python'%}

    User.query.filter(User.name!='wang').all()
    
{%endace%}

![逻辑非](../assets/logic_not.png)

### not\_ 相当于取反

{%ace edit=true, lang='python'%}

    from sqlalchemy import not_
    User.query.filter(not_(User.name=='chen')).all()
    
{%endace%}

![取反](../assets/not.png)

### 逻辑与，需要导入and，返回and\(\)条件满足的所有数据

{%ace edit=true, lang='python'%}

    from sqlalchemy import and_
    User.query.filter(and_(User.name!='wang',User.email.endswith('163.com'))).all()
    
{%endace%}

![逻辑与](../assets/logic_and.png)

### 逻辑或，需要导入or\_

{%ace edit=true, lang='python'%}

    from sqlalchemy import or_
    User.query.filter(or_(User.name!='wang',User.email.endswith('163.com'))).all()
    
{%endace%}

![逻辑或](../assets/logic_or.png)

### 查询数据后删除

{%ace edit=true, lang='python'%}

    user = User.query.first()
    db.session.delete(user)
    db.session.commit()
    User.query.all()
    
{%endace%}

### 更新数据

{%ace edit=true, lang='python'%}

    user = User.query.first()
    user.name = 'dong'
    db.session.commit()
    User.query.first()
    
{%endace%}

![更新数据](../assets/update_data.png)

### 关联查询示例：

> 角色和用户的关系是一对多的关系，一个角色可以有多个用户，一个用户只能属于一个角色。

  * 查询角色的所有用户

{%ace edit=true, lang='python'%}

    #查询roles表id为1的角色
    ro1 = Role.query.get(1)
    #查询该角色的所有用户
    ro1.us.all()
    
{%endace%}

![查询角色的所有用户](../assets/role_query_all.png)

  * 查询用户所属角色

{%ace edit=true, lang='python'%}

    #查询users表id为3的用户
    us1 = User.query.get(3)
    #查询用户属于什么角色
    us1.role
    
{%endace%}

![查询用户所属角色](../assets/user_in_role.png)

____

