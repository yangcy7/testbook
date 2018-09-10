# 过滤器

过滤器的本质就是函数。有时候我们不仅仅只是需要输出变量的值，我们还需要修改变量的显示，甚至格式化、运算等等，而在模板中是不能直接调用 Python
中的某些方法，那么这就用到了过滤器。

使用方式：

  * 过滤器的使用方式为：变量名 | 过滤器。

{%ace edit=true, lang='python'%}

    {{variable | filter_name(*args)}}
    
{%endace%}

  * 如果没有任何参数传给过滤器,则可以把括号省略掉

{%ace edit=true, lang='python'%}

    {{variable | filter_name}}
    
{%endace%}

  * 如：\`\`，这个过滤器的作用：把变量variable 的值的首字母转换为大写，其他字母转换为小写

## 链式调用

在 jinja2 中，过滤器是可以支持链式调用的，示例如下：

{%ace edit=true, lang='python'%}

    {{ "hello world" | reverse | upper }}
    
{%endace%}

## 常见内建过滤器

### 字符串操作

  * safe：禁用转义

{%ace edit=true, lang='python'%}

    &lt;p&gt;{{ '&lt;em&gt;hello&lt;/em&gt;' | safe }}&lt;/p&gt;
    
{%endace%}

  * capitalize：把变量值的首字母转成大写，其余字母转小写

{%ace edit=true, lang='python'%}

    &lt;p&gt;{{ 'hello' | capitalize }}&lt;/p&gt;
    
{%endace%}

  * lower：把值转成小写

{%ace edit=true, lang='python'%}

    &lt;p&gt;{{ 'HELLO' | lower }}&lt;/p&gt;
    
{%endace%}

  * upper：把值转成大写

{%ace edit=true, lang='python'%}

    &lt;p&gt;{{ 'hello' | upper }}&lt;/p&gt;
    
{%endace%}

  * title：把值中的每个单词的首字母都转成大写

{%ace edit=true, lang='python'%}

    &lt;p&gt;{{ 'hello' | title }}&lt;/p&gt;
    
{%endace%}

  * reverse：字符串反转

{%ace edit=true, lang='python'%}

    &lt;p&gt;{{ 'olleh' | reverse }}&lt;/p&gt;
    
{%endace%}

  * format：格式化输出

{%ace edit=true, lang='python'%}

    &lt;p&gt;{{ '%s is %d' | format('name',17) }}&lt;/p&gt;
    
{%endace%}

  * striptags：渲染之前把值中所有的HTML标签都删掉

{%ace edit=true, lang='python'%}

    &lt;p&gt;{{ '&lt;em&gt;hello&lt;/em&gt;' | striptags }}&lt;/p&gt;
    
{%endace%}

  * truncate: 字符串截断

{%ace edit=true, lang='python'%}

    &lt;p&gt;{{ 'hello every one' | truncate(9)}}&lt;/p&gt;
    
{%endace%}

### 列表操作

  * first：取第一个元素

{%ace edit=true, lang='python'%}

    &lt;p&gt;{{ [1,2,3,4,5,6] | first }}&lt;/p&gt;
    
{%endace%}

  * last：取最后一个元素

{%ace edit=true, lang='python'%}

    &lt;p&gt;{{ [1,2,3,4,5,6] | last }}&lt;/p&gt;
    
{%endace%}

  * length：获取列表长度

{%ace edit=true, lang='python'%}

    &lt;p&gt;{{ [1,2,3,4,5,6] | length }}&lt;/p&gt;
    
{%endace%}

  * sum：列表求和

{%ace edit=true, lang='python'%}

    &lt;p&gt;{{ [1,2,3,4,5,6] | sum }}&lt;/p&gt;
    
{%endace%}

  * sort：列表排序

{%ace edit=true, lang='python'%}

    &lt;p&gt;{{ [6,2,3,1,5,4] | sort }}&lt;/p&gt;
    
{%endace%}

### 语句块过滤

{%ace edit=true, lang='python'%}

    {% filter upper %}
        #一大堆文字#
    {% endfilter %}
    
{%endace%}

____

