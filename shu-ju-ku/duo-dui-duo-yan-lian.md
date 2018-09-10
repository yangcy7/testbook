# 多对多演练

在项目开发过程中，会遇到很多数据之间多对多关系的情况，比如：

  * 学生网上选课\(学生和课程\)
  * 老师与其授课的班级\(老师和班级\)
  * 用户与其收藏的新闻\(用户和新闻\)
  * 等等...

所以在开发过程中需要使用 ORM 模型将表与表的多对多关联关系使用代码描述出来。多对多关系描述有一个唯一的点就是： **需
要添加一张单独的表去记录两张表之间的对应关系**

## 场景示例

### 需求分析

  * 学生可以网上选课，学生有多个，课程也有多个
  * 学生有：张三、李四、王五
  * 课程有：物理、化学、生物
  * 选修关系有：
    * 张三选修了化学和生物
    * 李四选修了化学
    * 王五选修了物理、化学和生物
  * 需求：
    1. 查询某个学生选修了哪些课程
    2. 查询某个课程都有哪些学生选择

### 思路分析

  * 可以通过分析得出
    * 用一张表来保存所有的学生数据
    * 用一张表来保存所有的课程数据
  * 具体表及测试数据可以如下：

##### 学生表\(Student\)  
  
<table>  
<tr>  
<th>

主键\(id\)

</th>  
<th>

学生名\(name\)

</th> </tr>  
<tr>  
<td>

1

</td>  
<td>

张三

</td> </tr>  
<tr>  
<td>

2

</td>  
<td>

李四

</td> </tr>  
<tr>  
<td>

3

</td>  
<td>

王五

</td> </tr> </table>

##### 选修课表\(Course\)  
  
<table>  
<tr>  
<th>

主键\(id\)

</th>  
<th>

课程名\(name\)

</th> </tr>  
<tr>  
<td>

1

</td>  
<td>

物理

</td> </tr>  
<tr>  
<td>

2

</td>  
<td>

化学

</td> </tr>  
<tr>  
<td>

3

</td>  
<td>

生物

</td> </tr> </table>

##### 数据关联关系表\(Student\_Course\)  
  
<table>  
<tr>  
<th>

主键\(student.id\)

</th>  
<th>

主键\(course.id\)

</th> </tr>  
<tr>  
<td>

1

</td>  
<td>

2

</td> </tr>  
<tr>  
<td>

1

</td>  
<td>

3

</td> </tr>  
<tr>  
<td>

2

</td>  
<td>

2

</td> </tr>  
<tr>  
<td>

3

</td>  
<td>

1

</td> </tr>  
<tr>  
<td>

3

</td>  
<td>

2

</td> </tr>  
<tr>  
<td>

3

</td>  
<td>

3

</td> </tr> </table>

### 结果

  * 查询某个学生选修了哪些课程，例如：查询王五选修了哪些课程

    * 取出王五的 id 去 Student\_Course 表中查询 student.id 值为 3 的所有数据
    * 查询出来有3条数据，然后将这3条数据里面的 course.id 取值并查询 Course 表即可获得结果
  * 查询某个课程都有哪些学生选择，例如：查询生物课程都有哪些学生选修

    * 取出生物课程的 id 去 Student\_Course 表中查询 course.id 值为 3 的所有数据
    * 查询出来有2条数据，然后将这2条数据里面的 student.id 取值并查询 Student 表即可获得结果

## 代码演练

  * 定义模型及表

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
                                  backref='student',
                                  lazy='dynamic')
    
    
    class Course(db.Model):
        __tablename__ = "courses"
        id = db.Column(db.Integer, primary_key=True)
        name = db.Column(db.String(64), unique=True)
    
{%endace%}

  * 添加测试数据

{%ace edit=true, lang='python'%}

    if __name__ == '__main__':
        db.drop_all()
        db.create_all()
    
        # 添加测试数据
    
        stu1 = Student(name='张三')
        stu2 = Student(name='李四')
        stu3 = Student(name='王五')
    
        cou1 = Course(name='物理')
        cou2 = Course(name='化学')
        cou3 = Course(name='生物')
    
        stu1.courses = [cou2, cou3]
        stu2.courses = [cou2]
        stu3.courses = [cou1, cou2, cou3]
    
        db.session.add_all([stu1, stu2, stu2])
        db.session.add_all([cou1, cou2, cou3])
    
        db.session.commit()
    
        app.run(debug=True)
    
{%endace%}

____

