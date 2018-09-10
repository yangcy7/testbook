# 常见关系模板代码

以下罗列了使用关系型数据库中常见关系定义模板代码

## 一对多

  * 示例场景：
    * 用户与其发布的帖子\(用户表与帖子表\)
    * 角色与所属于该角色的用户\(角色表与多用户表\)
  * 示例代码

{%ace edit=true, lang='python'%}

    class Role(db.Model):
        """角色表"""
        __tablename__ = 'roles'
    
        id = db.Column(db.Integer, primary_key=True)
        name = db.Column(db.String(64), unique=True)
        users = db.relationship('User', backref='role', lazy='dynamic')
    
    class User(db.Model):
        """用户表"""
        __tablename__ = 'users'
        id = db.Column(db.Integer, primary_key=True)
        name = db.Column(db.String(64), unique=True, index=True)
    
{%endace%}

## 多对多

  * 示例场景
    * 讲师与其上课的班级\(讲师表与班级表\)
    * 用户与其收藏的新闻\(用户表与新闻表\)
    * 学生与其选修的课程\(学生表与选修课程表\)
  * 示例代码

{%ace edit=true, lang='python'%}

    tb_student_course = db.Table('tb_student_course',
                                 db.Column('student_id', db.Integer, db.ForeignKey('students.id')),
                                 db.Column('course_id', db.Integer, db.ForeignKey('courses.id'))
                                 )
    
    class Student(db.Model):
        __tablename__ = "students"
        id = db.Column(db.Integer, primary_key=True)
        name = db.Column(db.String(64), unique=True)
    
        courses = db.relationship('Course', secondary=tb_student_course,
                                  backref=db.backref('students', lazy='dynamic'),
                                  lazy='dynamic')
    
    class Course(db.Model):
        __tablename__ = "courses"
        id = db.Column(db.Integer, primary_key=True)
        name = db.Column(db.String(64), unique=True)
    
{%endace%}

## 自关联一对多

  * 示例场景
    * 评论与该评论的子评论\(评论表\)
    * 参考网易新闻
  * 示例代码

{%ace edit=true, lang='python'%}

    class Comment(db.Model):
        """评论"""
        __tablename__ = "comments"
    
        id = db.Column(db.Integer, primary_key=True)
        # 评论内容
        content = db.Column(db.Text, nullable=False)
        # 父评论id
        parent_id = db.Column(db.Integer, db.ForeignKey("comments.id"))
        # 父评论(也是评论模型)
        parent = db.relationship("Comment", remote_side=[id],
                                 backref=db.backref('childs', lazy='dynamic'))
    
    # 测试代码
    if __name__ == '__main__':
        db.drop_all()
        db.create_all()
    
        com1 = Comment(content='我是主评论1')
        com2 = Comment(content='我是主评论2')
        com11 = Comment(content='我是回复主评论1的子评论1')
        com11.parent = com1
        com12 = Comment(content='我是回复主评论1的子评论2')
        com12.parent = com1
    
        db.session.add_all([com1, com2, com11, com12])
        db.session.commit()
        app.run(debug=True)
    
{%endace%}

## 自关联多对多

  * 示例场景

    * 用户关注其他用户\(用户表，中间表\)
  * 示例代码

{%ace edit=true, lang='python'%}

    tb_user_follows = db.Table(
        "tb_user_follows",
        db.Column('follower_id', db.Integer, db.ForeignKey('info_user.id'), primary_key=True),  # 粉丝id
        db.Column('followed_id', db.Integer, db.ForeignKey('info_user.id'), primary_key=True)  # 被关注人的id
    )
    
    class User(db.Model):
        """用户表"""
        __tablename__ = "info_user"
    
        id = db.Column(db.Integer, primary_key=True)  
        name = db.Column(db.String(32), unique=True, nullable=False)
    
        # 用户所有的粉丝，添加了反向引用followed，代表用户都关注了哪些人
        followers = db.relationship('User',
                                    secondary=tb_user_follows,
                                    primaryjoin=id == tb_user_follows.c.followed_id,
                                    secondaryjoin=id == tb_user_follows.c.follower_id,
                                    backref=db.backref('followed', lazy='dynamic'),
                                    lazy='dynamic')
    
{%endace%}

____

