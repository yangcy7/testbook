# 宏

对宏\(macro\)的理解：

  * 把它看作 Jinja2 中的一个函数，它会返回一个模板或者 HTML 字符串
  * 为了避免反复地编写同样的模板代码，出现代码冗余，可以把他们写成函数以进行重用
  * 需要在多处重复使用的模板代码片段可以写入单独的文件，再包含在所有模板中，以避免重复

## 使用

  * 定义宏

{%ace edit=true, lang='python'%}

    {% macro input(name,value='',type='text') %}
        &lt;input type="{{type}}" name="{{name}}"
            value="{{value}}" class="form-control"&gt;
    {% endmacro %}
    
{%endace%}

  * 调用宏

{%ace edit=true, lang='python'%}

    {{ input('name' value='zs')}}
    
{%endace%}

  * 这会输出

{%ace edit=true, lang='python'%}

    &lt;input type="text" name="name"
        value="zs" class="form-control"&gt;
    
{%endace%}

  * 把宏单独抽取出来，封装成html文件，其它模板中导入使用，文件名可以自定义macro.html

{%ace edit=true, lang='python'%}

    {% macro function(type='text', name='', value='') %}
    &lt;input type="{{type}}" name="{{name}}"
    value="{{value}}" class="form-control"&gt;
    
    {% endmacro %}
    
{%endace%}

  * 在其它模板文件中先导入，再调用

{%ace edit=true, lang='python'%}

    {% import 'macro.html' as func %}
    {% func.function() %}
    
{%endace%}

## 代码演练

  * 使用宏之前代码

{%ace edit=true, lang='python'%}

    &lt;form&gt;
        &lt;label&gt;用户名：&lt;/label&gt;&lt;input type="text" name="username"&gt;&lt;br/&gt;
        &lt;label&gt;身份证号：&lt;/label&gt;&lt;input type="text" name="idcard"&gt;&lt;br/&gt;
        &lt;label&gt;密码：&lt;/label&gt;&lt;input type="password" name="password"&gt;&lt;br/&gt;
        &lt;label&gt;确认密码：&lt;/label&gt;&lt;input type="password" name="password2"&gt;&lt;br/&gt;
        &lt;input type="submit" value="注册"&gt;
    &lt;/form&gt;
    
{%endace%}

  * 定义宏

{%ace edit=true, lang='python'%}

    {#定义宏，相当于定义一个函数，在使用的时候直接调用该宏，传入不同的参数就可以了#}
    {% macro input(label="", type="text", name="", value="") %}
    &lt;label&gt;{{ label }}&lt;/label&gt;&lt;input type="{{ type }}" name="{{ name }}" value="{{ value }}"&gt;
    {% endmacro %}
    
{%endace%}

  * 使用宏

{%ace edit=true, lang='python'%}

    &lt;form&gt;
        {{ input("用户名：", name="username") }}&lt;br/&gt;
        {{ input("身份证号：", name="idcard") }}&lt;br/&gt;
        {{ input("密码：", type="password", name="password") }}&lt;br/&gt;
        {{ input("确认密码：", type="password", name="password2") }}&lt;br/&gt;
        {{ input(type="submit", value="注册") }}
    &lt;/form&gt;
    
{%endace%}

____

