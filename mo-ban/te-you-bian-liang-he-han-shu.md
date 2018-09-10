# 模板中特有的变量和函数

你可以在自己的模板中访问一些 Flask 默认内置的函数和对象

#### config

你可以从模板中直接访问Flask当前的config对象:

{%ace edit=true, lang='python'%}

    {{config.SQLALCHEMY_DATABASE_URI}}
    sqlite:///database.db
    
{%endace%}

#### request

就是flask中代表当前请求的request对象：

{%ace edit=true, lang='python'%}

    {{request.url}}
    http://127.0.0.1
    
{%endace%}

#### session

为Flask的session对象

{%ace edit=true, lang='python'%}

    {{session.new}}
    True
    
{%endace%}

#### g变量

在视图函数中设置g变量的 name 属性的值，然后在模板中直接可以取出

{%ace edit=true, lang='python'%}

    {{ g.name }}
    
{%endace%}

#### url\_for\(\)

url\_for会根据传入的路由器函数名,返回该路由对应的URL,在模板中始终使用url\_for\(\)就可以安全的修改路由绑定的URL,则不比担心模板中渲染出错的链接:

{%ace edit=true, lang='python'%}

    {{url_for('home')}}
    /
    
{%endace%}

如果我们定义的路由URL是带有参数的,则可以把它们作为关键字参数传入url\_for\(\),Flask会把他们填充进最终生成的URL中:

{%ace edit=true, lang='python'%}

    {{ url_for('post', post_id=1)}}
    /post/1
    
{%endace%}

##### get\_flashed\_messages\(\)

这个函数会返回之前在flask中通过flask\(\)传入的消息的列表，flash函数的作用很简单,可以把由Python字符串表示的消息加入一个消息队列中，再使用get\_flashed\_message\(\)函数取出它们并消费掉：

{%ace edit=true, lang='python'%}

    {%for message in get_flashed_messages()%}
        {{message}}
    {%endfor%}
    
{%endace%}

____

