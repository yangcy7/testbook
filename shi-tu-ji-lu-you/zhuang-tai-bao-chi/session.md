# Session

  * 对于敏感、重要的信息，建议要存储在服务器端，不能存储在浏览器中，如用户名、余额、等级、验证码等信息
  * 在服务器端进行状态保持的方案就是`Session`
  * **Session 依赖于Cookie**

## session数据的获取

session:请求上下文对象，用于处理http请求中的一些数据内容

{%ace edit=true, lang='python'%}

    @app.route('/index1')
    def index1():
        session['username'] = 'itcast'
        return redirect(url_for('index'))
    @app.route('/')
    def index():
        return session.get('username')
    
{%endace%}

> 记得设置secret\_key: app.secret\_key = 'itheima'
secret\_key的作用：https://segmentfault.com/q/1010000007295395

____

