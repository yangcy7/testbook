# 自定义过滤器

过滤器的本质是函数。当模板内置的过滤器不能满足需求，可以自定义过滤器。自定义过滤器有两种实现方式：

  * 一种是通过Flask应用对象的 **add\_template\_filter** 方法
  * 通过装饰器来实现自定义过滤器

**重 要：自定义的过滤器名称如果和内置的过滤器重名，会覆盖内置的过滤器。**

### 需求：添加列表反转的过滤器

#### 方式一

通过调用应用程序实例的 add\_template\_filter 方法实现自定义过滤器。该方法第一个参数是函数名，第二个参数是自定义的过滤器名称：

{%ace edit=true, lang='python'%}

    def do_listreverse(li):
        # 通过原列表创建一个新列表
        temp_li = list(li)
        # 将新列表进行返转
        temp_li.reverse()
        return temp_li
    
    app.add_template_filter(do_listreverse,'lireverse')
    
{%endace%}

#### 方式二

用装饰器来实现自定义过滤器。装饰器传入的参数是自定义的过滤器名称。

{%ace edit=true, lang='python'%}

    @app.template_filter('lireverse')
    def do_listreverse(li):
        # 通过原列表创建一个新列表
        temp_li = list(li)
        # 将新列表进行返转
        temp_li.reverse()
        return temp_li
    
{%endace%}

  * 在 html 中使用该自定义过滤器

{%ace edit=true, lang='python'%}

    &lt;br/&gt; my_array 原内容：{{ my_array }}
    &lt;br/&gt; my_array 反转：{{ my_array | lireverse }}
    
{%endace%}

  * 运行结果

{%ace edit=true, lang='python'%}

    my_array 原内容：[3, 4, 2, 1, 7, 9] 
    my_array 反转：[9, 7, 1, 2, 4, 3]
    
{%endace%}

____

