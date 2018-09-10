# Flask简介

## Web应用程序的本质

Web\(World Wide Web\) 诞生最初的目的，是为了利用互联网交流工作文档。

![](../assets/http_request.png)

## Web框架

  * 什么是Web框架？
    * 协助开发者快速开发 Web 应用程序的一套功能代码
    * 开发者只需要按照框架约定要求，在指定位置写上自己的业务逻辑代码
    * 例如：在某个区需要成立一家医院，有两种方式：
      1. 圈地，打地基，盖楼，装修，入驻
      2. 买楼，装修，入驻
  * 为什么要用Web框架？

![](../assets/car.png)

web网站发展至今，特别是服务器端，涉及到的知识、内容，非常广泛。这对程序员的要求会越来越高。如果采用成熟，稳健的框架，那么一些基础的工作，比如，安全性，数据流控制等都可以让框架来处理，那么程序开发人员可以把精力放在具体的业务逻辑上面。使用框架的优点：

  * 稳定性和可扩展性强
  * 可以降低开发难度，提高开发效率。

总结一句话： **避 免重复造轮子**

  * 在 Python 中常用的 Web 框架有
    * flask
    * django
    * tornado

## Flask

Flask诞生于2010年，是Armin ronacher（人名）用 Python 语言基于 Werkzeug 工具箱编写的轻量级Web开发框架。

Flask 本身相当于一个内核，其他几乎所有的功能都要用到扩展（邮件扩展Flask-Mail，用户认证Flask-Login，数据库Flask-
SQLAlchemy），都需要用第三方的扩展来实现。比如可以用 Flask 扩展加入ORM、窗体验证工具，文件上传、身份验证等。Flask
没有默认使用的数据库，你可以选择 MySQL，也可以用 NoSQL。

其 WSGI 工具箱采用 Werkzeug（路由模块），模板引擎则使用 Jinja2。这两个也是 Flask 框架的核心。

**Flask 常用扩展包：**

  * Flask-SQLalchemy：操作数据库；
  * Flask-script：插入脚本；
  * Flask-migrate：管理迁移数据库；
  * Flask-Session：Session存储方式指定；
  * Flask-WTF：表单；
  * Flask-Mail：邮件；
  * Flask-Bable：提供国际化和本地化支持，翻译；
  * Flask-Login：认证用户状态；
  * Flask-OpenID：认证；
  * Flask-RESTful：开发REST API的工具；
  * Flask-Bootstrap：集成前端Twitter Bootstrap框架；
  * Flask-Moment：本地化日期和时间；
  * Flask-Admin：简单而可扩展的管理接口的框架

> 扩展列表：http://flask.pocoo.org/extensions/

  1. 中文文档（http://docs.jinkan.org/docs/flask/）
  2. 英文文档（http://flask.pocoo.org/docs/0.11/）

____

