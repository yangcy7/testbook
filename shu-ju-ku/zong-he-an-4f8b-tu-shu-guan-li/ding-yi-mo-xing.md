# 定义模型

模型表示程序使用的数据实体，在Flask-
SQLAlchemy中，模型一般是Python类，继承自db.Model，db是SQLAlchemy类的实例，代表程序使用的数据库。

类中的属性对应数据库表中的列。id为主键，是由Flask-SQLAlchemy管理。db.Column类构造函数的第一个参数是数据库列和模型属性类型。

> 注：如果没有在创建数据库的时候指定编码的话，向数据库中插入中文后，会报错，那么需要修改数据库的编码集:

{%ace edit=true, lang='python'%}

    alter database 数据库名 CHARACTER SET utf8
    
{%endace%}

如下示例：定义了两个模型类，作者和书名。

{%ace edit=true, lang='python'%}

    #coding=utf-8
    from flask import Flask,render_template,redirect,url_for
    from flask_sqlalchemy import SQLAlchemy
    
    app = Flask(__name__)
    
    #设置连接数据
    app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:mysql@127.0.0.1:3306/test2'
    
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True
    
    #实例化SQLAlchemy对象
    db = SQLAlchemy(app)
    
    #定义模型类-作者
    class Author(db.Model):
        __tablename__ = 'author'
        id = db.Column(db.Integer,primary_key=True)
        name = db.Column(db.String(32),unique=True
        au_book = db.relationship('Book',backref='author')
        def __repr__(self):
            return 'Author:%s' %self.name
    
    #定义模型类-书名
    class Book(db.Model):
        __tablename__ = 'books'
        id = db.Column(db.Integer,primary_key=True)
        name = db.Column(db.String(32))
        au_book = db.Column(db.Integer,db.ForeignKey('author.id'))
        def __str__(self):
            return 'Book:%s,%s'%(self.info,self.lead)
    
{%endace%}

____

