# 视图常用逻辑

  * 返回 JSON
  * 重定向
    * url\_for
  * 自定义状态码

## 返回JSON

在使用 Flask 写一个接口时候需要给客户端返回 JSON 数据，在 Flask 中可以直接使用 **jsonify** 生成一个 JSON 的响应

{%ace edit=true, lang='python'%}

    # 返回JSON
    @app.route('/demo4')
    def demo4():
        json_dict = {
            "user_id": 10,
            "user_name": "laowang"
        }
        return jsonify(json_dict)
    
{%endace%}

> 不推荐使用 json.dumps 转成 JSON 字符串直接返回，因为返回的数据要符合 HTTP 协议规范，如果是 JSON 需要指定 content-
type:application/json

## 重定向

  * 重定向到 **黑 马** 官网

{%ace edit=true, lang='python'%}

    # 重定向
    @app.route('/demo5')
    def demo5():
        return redirect('http://www.itheima.com')
    
{%endace%}

  * 重定向到自己写的视图函数
    * 可以直接填写自己 url 路径
    * 也可以使用 url\_for 生成指定视图函数所对应的 url

{%ace edit=true, lang='python'%}

    @app.route('/demo1')
    def demo1():
        return 'demo1'
    
    # 重定向
    @app.route('/demo5')
    def demo5():
        return redirect(url_for('demo1'))
    
{%endace%}

  * 重定向到带有参数的视图函数
    * 在 url\_for 函数中传入参数

{%ace edit=true, lang='python'%}

    # 路由传递参数
    @app.route('/user/&lt;int:user_id&gt;')
    def user_info(user_id):
        return 'hello %d' % user_id
    
    # 重定向
    @app.route('/demo5')
    def demo5():
        # 使用 url_for 生成指定视图函数所对应的 url
        return redirect(url_for('user_info', user_id=100))
    
{%endace%}

## 自定义状态码

  * 在 Flask 中，可以很方便的返回自定义状态码，以实现不符合 http 协议的状态码，例如：status code: 666

{%ace edit=true, lang='python'%}

    @app.route('/demo6')
    def demo6():
        return '状态码为 666', 666
    
{%endace%}

____

