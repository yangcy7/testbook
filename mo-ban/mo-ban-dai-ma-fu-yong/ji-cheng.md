# 模板继承

模板继承是为了重用模板中的公共内容。一般Web开发中，继承主要使用在网站的顶部菜单、底部。这些内容可以定义在父模板中，子模板直接继承，而不需要重复书写。

  * 标签定义的内容

{%ace edit=true, lang='python'%}

    {% block top %} {% endblock %}
    
{%endace%}

  * 相当于在父模板中挖个坑，当子模板继承父模板时，可以进行填充。
  * 子模板使用 extends 指令声明这个模板继承自哪个模板
  * 父模板中定义的块在子模板中被重新定义，在子模板中调用父模板的内容可以使用super\(\)

## 父模板

  * base.html

{%ace edit=true, lang='python'%}

    {% block top %}
      顶部菜单
    {% endblock top %}
    
    {% block content %}
    {% endblock content %}
    
    {% block bottom %}
      底部
    {% endblock bottom %}
    
{%endace%}

## 子模板

  * extends指令声明这个模板继承自哪

{%ace edit=true, lang='python'%}

    {% extends 'base.html' %}
    {% block content %}
     需要填充的内容
    {% endblock content %}
    
{%endace%}

  * 模板继承使用时注意点：
    * 不支持多继承
    * 为了便于阅读，在子模板中使用extends时，尽量写在模板的第一行。
    * 不能在一个模板文件中定义多个相同名字的block标签。
    * 当在页面中使用多个block标签时，建议给结束标签起个名字，当多个block嵌套时，阅读性更好。

____

