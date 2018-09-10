# Web表单

Web 表单是 Web 应用程序的基本功能。

它是HTML页面中负责数据采集的部件。表单有三个部分组成：表单标签、表单域、表单按钮。表单允许用户输入数据，负责HTML页面数据采集，通过表单将用户输入的数据提交给服务器。

在Flask中，为了处理web表单，我们可以使用 Flask-WTF 扩展，它封装了 WTForms，并且它有验证表单数据的功能

## WTForms支持的HTML标准字段  
  
<table>  
<tr>  
<th>

字段对象

</th>  
<th>

说明

</th> </tr>  
<tr>  
<td>

StringField

</td>  
<td>

文本字段

</td> </tr>  
<tr>  
<td>

TextAreaField

</td>  
<td>

多行文本字段

</td> </tr>  
<tr>  
<td>

PasswordField

</td>  
<td>

密码文本字段

</td> </tr>  
<tr>  
<td>

HiddenField

</td>  
<td>

隐藏文件字段

</td> </tr>  
<tr>  
<td>

DateField

</td>  
<td>

文本字段，值为 datetime.date 文本格式

</td> </tr>  
<tr>  
<td>

DateTimeField

</td>  
<td>

文本字段，值为 datetime.datetime 文本格式

</td> </tr>  
<tr>  
<td>

IntegerField

</td>  
<td>

文本字段，值为整数

</td> </tr>  
<tr>  
<td>

DecimalField

</td>  
<td>

文本字段，值为decimal.Decimal

</td> </tr>  
<tr>  
<td>

FloatField

</td>  
<td>

文本字段，值为浮点数

</td> </tr>  
<tr>  
<td>

BooleanField

</td>  
<td>

复选框，值为True 和 False

</td> </tr>  
<tr>  
<td>

RadioField

</td>  
<td>

一组单选框

</td> </tr>  
<tr>  
<td>

SelectField

</td>  
<td>

下拉列表

</td> </tr>  
<tr>  
<td>

SelectMutipleField

</td>  
<td>

下拉列表，可选择多个值

</td> </tr>  
<tr>  
<td>

FileField

</td>  
<td>

文件上传字段

</td> </tr>  
<tr>  
<td>

SubmitField

</td>  
<td>

表单提交按钮

</td> </tr>  
<tr>  
<td>

FormField

</td>  
<td>

把表单作为字段嵌入另一个表单

</td> </tr>  
<tr>  
<td>

FieldList

</td>  
<td>

一组指定类型的字段

</td> </tr> </table>

## WTForms常用验证函数  
  
<table>  
<tr>  
<th>

验证函数

</th>  
<th>

说明

</th> </tr>  
<tr>  
<td>

DataRequired

</td>  
<td>

确保字段中有数据

</td> </tr>  
<tr>  
<td>

EqualTo

</td>  
<td>

比较两个字段的值，常用于比较两次密码输入

</td> </tr>  
<tr>  
<td>

Length

</td>  
<td>

验证输入的字符串长度

</td> </tr>  
<tr>  
<td>

NumberRange

</td>  
<td>

验证输入的值在数字范围内

</td> </tr>  
<tr>  
<td>

URL

</td>  
<td>

验证URL

</td> </tr>  
<tr>  
<td>

AnyOf

</td>  
<td>

验证输入值在可选列表中

</td> </tr>  
<tr>  
<td>

NoneOf

</td>  
<td>

验证输入值不在可选列表中

</td> </tr> </table>

使用 Flask-WTF 需要配置参数 SECRET\_KEY。

CSRF\_ENABLED是为了CSRF（跨站请求伪造）保护。
SECRET\_KEY用来生成加密令牌，当CSRF激活的时候，该设置会根据设置的密匙生成加密令牌。

## 代码验证

### 使用 html 自带的表单

  * 创建模板文件 `login.html`，在其中直接写form表单：

{%ace edit=true, lang='python'%}

    &lt;form method="post"&gt;
        &lt;label&gt;用户名：&lt;/label&gt;&lt;input type="text" name="username" placeholder="请输入用户名"&gt;&lt;br/&gt;
        &lt;label&gt;密码：&lt;/label&gt;&lt;input type="password" name="password" placeholder="请输入密码"&gt;&lt;br/&gt;
        &lt;label&gt;确认密码：&lt;/label&gt;&lt;input type="password" name="password2" placeholder="请输入确认密码"&gt;&lt;br/&gt;
        &lt;input type="submit" value="注册"&gt;
    &lt;/form&gt;
    
    {% for message in get_flashed_messages() %}
        {{ message }}
    {% endfor %}
    
{%endace%}

  * 视图函数中获取表单数据验证登录逻辑：

{%ace edit=true, lang='python'%}

    @app.route('/demo1', methods=["get", "post"])
    def demo1():
        if request.method == "POST":
            # 取到表单中提交上来的三个参数
            username = request.form.get("username")
            password = request.form.get("password")
            password2 = request.form.get("password2")
    
            if not all([username, password, password2]):
                # 向前端界面弹出一条提示(闪现消息)
                flash("参数不足")
            elif password != password2:
                flash("两次密码不一致")
            else:
                # 假装做注册操作
                print(username, password, password2)
                return "success"
    
        return render_template('temp_register.html')
    
{%endace%}

### 使用 Flask-WTF 实现表单

  * 配置参数，关闭 CSRF 校验

{%ace edit=true, lang='python'%}

     app.config['WTF_CSRF_ENABLED'] = False
    
{%endace%}

> CSRF:跨站请求伪造，后续会讲到

#### 模板页面：

{%ace edit=true, lang='python'%}

    &lt;form method="post"&gt;
        {{ form.username.label }} {{ form.username }}&lt;br/&gt;
        {{ form.password.label }} {{ form.password }}&lt;br/&gt;
        {{ form.password2.label }} {{ form.password2 }}&lt;br/&gt;
        {{ form.submit }}
    &lt;/form&gt;
    
{%endace%}

#### 视图函数：

{%ace edit=true, lang='python'%}

    from flask import Flask,render_template, flash
    #导入wtf扩展的表单类
    from flask_wtf import FlaskForm
    #导入自定义表单需要的字段
    from wtforms import SubmitField,StringField,PasswordField
    #导入wtf扩展提供的表单验证器
    from wtforms.validators import DataRequired,EqualTo
    
    
    app = Flask(__name__)
    app.config['SECRET_KEY']='SECRET_KEY'
    
    #自定义表单类，文本字段、密码字段、提交按钮
    class RegisterForm(FlaskForm):
        username = StringField("用户名：", validators=[DataRequired("请输入用户名")], render_kw={"placeholder": "请输入用户名"})
        password = PasswordField("密码：", validators=[DataRequired("请输入密码")])
        password2 = PasswordField("确认密码：", validators=[DataRequired("请输入确认密码"), EqualTo("password", "两次密码不一致")])
        submit = SubmitField("注册")
    
    #定义根路由视图函数，生成表单对象，获取表单数据，进行表单数据验证
    @app.route('/demo2', methods=["get", "post"])
    def demo2():
        register_form = RegisterForm()
        # 验证表单
        if register_form.validate_on_submit():
            # 如果代码能走到这个地方，那么就代码表单中所有的数据都能验证成功
            username = request.form.get("username")
            password = request.form.get("password")
            password2 = request.form.get("password2")
            # 假装做注册操作
            print(username, password, password2)
            return "success"
        else:
            if request.method == "POST":
                flash("参数有误或者不完整")
    
        return render_template('temp_register.html', form=register_form)
    
    if __name__ == '__main__':
        app.run(debug=True)
    
{%endace%}

> 运行测试

____

