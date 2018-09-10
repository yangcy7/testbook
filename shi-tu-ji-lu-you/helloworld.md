# HelloWorld程序

## 创建 Python 项目

  * 打开 Pycharm，创建 `Pure Python` 类型的项目，创建项目完成之后选择之前创建的 `py3_flask` 作为虚拟环境

![](../assets/选择Python解析器.png)

> 第 4 步路径可以通过在指定虚拟环境下，输入 `which python` 获得

## 示例：

  * 新建文件helloworld.py
  * 导入Flask类

{%ace edit=true, lang='python'%}

    from flask import Flask
    
{%endace%}

Flask函数接收一个参数\_\_name\_\_，它会指向程序所在的包

{%ace edit=true, lang='python'%}

    app = Flask(__name__)
    
{%endace%}

  * 装饰器的作用是将路由映射到视图函数 index

{%ace edit=true, lang='python'%}

    @app.route('/')
    def index():
        return 'Hello World'
    
{%endace%}

  * Flask应用程序实例的 run 方法 启动 WEB 服务器

{%ace edit=true, lang='python'%}

    if __name__ == '__main__':
        app.run()
    
{%endace%}

  * 在程序运行过程中，程序实例中会使用 `url_map` 将装饰器路由和视图的对应关系保存起来，打印结果如下图：

![](../assets/app_route_py3.png)

____

