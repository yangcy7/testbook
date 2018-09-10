# 单元测试

### 为什么要测试？

Web程序开发过程一般包括以下几个阶段：\[需求分析，设计阶段，实现阶段，测试阶段\]。其中测试阶段通过人工或自动来运行测试某个系统的功能。目的是检验其是否满足需求，并得出特定的结果，以达到弄清楚预期结果和实际结果之间的差别的最终目的。

#### 测试的分类：

测试从软件开发过程可以分为：

  * 单元测试
    * 对单独的代码块\(例如函数\)分别进行测试,以保证它们的正确性
  * 集成测试
    * 对大量的程序单元的协同工作情况做测试
  * 系统测试
    * 同时对整个系统的正确性进行检查,而不是针对独立的片段

在众多的测试中，与程序开发人员最密切的就是单元测试，因为单元测试是由开发人员进行的，而其他测试都由专业的测试人员来完成。所以我们主要学习单元测试。

### 什么是单元测试？

程序开发过程中，写代码是为了实现需求。当我们的代码通过了编译，只是说明它的语法正确，功能能否实现则不能保证。
因此，当我们的某些功能代码完成后，为了检验其是否满足程序的需求。可以通过编写测试代码，模拟程序运行的过程，检验功能代码是否符合预期。

单元测试就是开发者编写一小段代码，检验目标代码的功能是否符合预期。通常情况下，单元测试主要面向一些功能单一的模块进行。

举个例子：一部手机有许多零部件组成，在正式组装一部手机前，手机内部的各个零部件，CPU、内存、电池、摄像头等，都要进行测试，这就是单元测试。

在Web开发过程中，单元测试实际上就是一些"断言"（assert）代码。

断言就是判断一个函数或对象的一个方法所产生的结果是否符合你期望的那个结果。
python中assert断言是声明布尔值为真的判定，如果表达式为假会发生异常。单元测试中，一般使用assert来断言结果。

#### 断言方法的使用：

![](../assets/断言.png)

断言语句类似于：

{%ace edit=true, lang='python'%}

    if not expression:    
        raise AssertionError
     AssertionError
    
{%endace%}

**常 用的断言方法：**

{%ace edit=true, lang='python'%}

    assertEqual     如果两个值相等，则pass
    assertNotEqual  如果两个值不相等，则pass
    assertTrue      判断bool值为True，则pass
    assertFalse     判断bool值为False，则pass
    assertIsNone    不存在，则pass
    assertIsNotNone 存在，则pass
    
{%endace%}

### 如何测试？

简单的测试用例：1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144,
233，377，610，987，1597，2584，4181，6765，

{%ace edit=true, lang='python'%}

    def fibo(x):
        if x == 0:
            resp = 0
        elif x == 1:
            resp = 1
        else:
            return fibo(x-1) + fibo(x-2)
        return resp
    assert fibo(5) == 5
    
{%endace%}

![fibo](../assets/fibo.png)

### 单元测试的基本写法：

**首 先**，定义一个类，继承自unittest.TestCase

{%ace edit=true, lang='python'%}

    import unittest
    class TestClass(unitest.TestCase):
        pass
    
{%endace%}

**其 次**，在测试类中，定义两个测试方法

{%ace edit=true, lang='python'%}

    import unittest
    class TestClass(unittest.TestCase):
    
        #该方法会首先执行，方法名为固定写法
        def setUp(self):
            pass
    
        #该方法会在测试代码执行完后执行，方法名为固定写法
        def tearDown(self):
            pass
    
{%endace%}

**最 后**，在测试类中，编写测试代码

{%ace edit=true, lang='python'%}

    import unittest
    class TestClass(unittest.TestCase):
    
        #该方法会首先执行，相当于做测试前的准备工作
        def setUp(self):
            pass
    
        #该方法会在测试代码执行完后执行，相当于做测试后的扫尾工作
        def tearDown(self):
            pass
        #测试代码
        def test_app_exists(self):
            pass
    
{%endace%}

### 登录测试

  * 被测试的代码逻辑

{%ace edit=true, lang='python'%}

    @app.route('/login', methods=['POST'])
    def login():
        username = request.form.get('username')
        password = request.form.get('password')
    
        # 判断参数是否为空
        if not all([username, password]):
            result = {
                "errcode": -2,
                "errmsg": "params error"
            }
            return jsonify(result)
    
        # a = 1 / 0
        # 如果账号密码正确
        # 判断账号密码是否正确
        if username == 'itheima' and password == 'python':
            result = {
                "errcode": 0,
                "errmsg": "success"
            }
            return jsonify(result)
        else:
            result = {
                "errcode": -1,
                "errmsg": "wrong username or password"
            }
            return jsonify(result)
    
{%endace%}

  * 单元测试代码

{%ace edit=true, lang='python'%}

    import json
    import unittest
    from demo1_login import app
    
    class LoginTest(unittest.TestCase):
        """为登录逻辑编写测试案例"""
    
        def setUp(self):
            app.testing = True
            self.client = app.test_client()
    
        def test_empty_username_password(self):
            """测试用户名与密码为空的情况[当参数不全的话，返回errcode=-2]"""
            response = app.test_client().post('/login', data={})
            json_data = response.data
            json_dict = json.loads(json_data)
    
            self.assertIn('errcode', json_dict, '数据格式返回错误')
            self.assertEqual(json_dict['errcode'], -2, '状态码返回错误')
    
            # TODO 测试用户名为空的情况
    
            # TODO 测试密码为空的情况
    
        def test_error_username_password(self):
            """测试用户名和密码错误的情况[当登录名和密码错误的时候，返回 errcode = -1]"""
            response = app.test_client().post('/login', data={"username": "aaaaa", "password": "12343"})
            json_data = response.data
            json_dict = json.loads(json_data)
            self.assertIn('errcode', json_dict, '数据格式返回错误')
            self.assertEqual(json_dict['errcode'], -1, '状态码返回错误')
    
            # TODO 测试用户名错误的情况
    
            # TODO 测试密码错误的情况
    
    if __name__ == '__main__':
        unittest.main()
    
{%endace%}

### 数据库测试：

{%ace edit=true, lang='python'%}

    #coding=utf-8
    import unittest
    from author_book import *
    
    #自定义测试类，setUp方法和tearDown方法会分别在测试前后执行。以test_开头的函数就是具体的测试代码。
    class DatabaseTestCase(unittest.TestCase):
        def setUp(self):
            app.config['TESTING'] = True
            app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:mysql@localhost/test0'
            self.app = app
            db.create_all()
    
        def tearDown(self):
            db.session.remove()
            db.drop_all()
    
        #测试代码
        def test_append_data(self):
            au = Author(name='itcast')
            bk = Book(info='python')
            db.session.add_all([au,bk])
            db.session.commit()
            author = Author.query.filter_by(name='itcast').first()
            book = Book.query.filter_by(info='python').first()
            #断言数据存在
            self.assertIsNotNone(author)
            self.assertIsNotNone(book)
    
{%endace%}

__

