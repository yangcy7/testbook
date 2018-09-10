# 正则匹配路由

在 web 开发中，可能会出现限制用户访问规则的场景，那么这个时候就需要用到正则匹配，根据自己的规则去限定请求参数再进行访问

具体实现步骤为：

  * 导入转换器基类：在 Flask 中，所有的路由的匹配规则都是使用转换器对象进行记录
  * 自定义转换器：自定义类继承于转换器基类
  * 添加转换器到默认的转换器字典中
  * 使用自定义转换器实现自定义匹配规则

## 代码实现

  * 导入转换器基类

{%ace edit=true, lang='python'%}

    from werkzeug.routing import BaseConverter
    
{%endace%}

  * 自定义转换器

{%ace edit=true, lang='python'%}

    # 自定义正则转换器
    class RegexConverter(BaseConverter):
        def __init__(self, url_map, *args):
            super(RegexConverter, self).__init__(url_map)
            # 将接受的第1个参数当作匹配规则进行保存
            self.regex = args[0]
    
{%endace%}

  * 添加转换器到默认的转换器字典中，并指定转换器使用时名字为: re

{%ace edit=true, lang='python'%}

    app = Flask(__name__)
    
    # 将自定义转换器添加到转换器字典中，并指定转换器使用时名字为: re
    app.url_map.converters['re'] = RegexConverter
    
{%endace%}

  * 使用转换器去实现自定义匹配规则
    * 当前此处定义的规则是：3位数字

{%ace edit=true, lang='python'%}

    @app.route('/user/<re("[0-9]{3}"):user_id>')
    def user_info(user_id):
        return "user_id 为 %s" % user_id
    
{%endace%}

> 运行测试：http://127.0.0.1:5000/user/123 ，如果访问的url不符合规则，会提示找不到页面

## 自定义转换器其他两个函数实现

继承于自定义转换器之后，还可以实现 to\_python 和 to\_url 这两个函数去对匹配参数做进一步处理：

  * to\_python：
    * 该函数参数中的 value 值代表匹配到的值，可输出进行查看
    * 匹配完成之后，对匹配到的参数作最后一步处理再返回，比如：转成 int 类型的值再返回：

{%ace edit=true, lang='python'%}

    class RegexConverter(BaseConverter):
        def __init__(self, url_map, *args):
            super(RegexConverter, self).__init__(url_map)
            # 将接受的第1个参数当作匹配规则进行保存
            self.regex = args[0]
    
        def to_python(self, value):
            return int(value)
    
{%endace%}

> 运行测试，在视图函数中可以查看参数的类型，由之前默认的 str 已变成 int 类型的值

  * to\_url:
    * 在使用 url\_for 去获取视图函数所对应的 url 的时候，会调用此方法对 url\_for 后面传入的视图函数参数做进一步处理
    * 具体可参见 Flask 的 app.py 中写的示例代码：ListConverter

# 系统自带转换器

{%ace edit=true, lang='python'%}

    DEFAULT_CONVERTERS = {
        'default':          UnicodeConverter,
        'string':           UnicodeConverter,
        'any':              AnyConverter,
        'path':             PathConverter,
        'int':              IntegerConverter,
        'float':            FloatConverter,
        'uuid':             UUIDConverter,
    }
    
{%endace%}

> 系统自带的转换器具体使用方式在每种转换器的注释代码中有写，请留意每种转换器初始化的参数。

____

