# request

request 就是flask中代表当前请求的 request 对象，其中一个请求上下文变量\(理解成全局变量，在视图函数中直接使用可以取到当前本次请求\)

常用的属性如下：  
  
<table>  
<tr>  
<th>

属性

</th>  
<th>

说明

</th>  
<th>

类型

</th> </tr>  
<tr>  
<td>

data

</td>  
<td>

记录请求的数据，并转换为字符串

</td>  
<td>

\*

</td> </tr>  
<tr>  
<td>

form

</td>  
<td>

记录请求中的表单数据

</td>  
<td>

MultiDict

</td> </tr>  
<tr>  
<td>

args

</td>  
<td>

记录请求中的查询参数

</td>  
<td>

MultiDict

</td> </tr>  
<tr>  
<td>

cookies

</td>  
<td>

记录请求中的cookie信息

</td>  
<td>

Dict

</td> </tr>  
<tr>  
<td>

headers

</td>  
<td>

记录请求中的报文头

</td>  
<td>

EnvironHeaders

</td> </tr>  
<tr>  
<td>

method

</td>  
<td>

记录请求使用的HTTP方法

</td>  
<td>

GET/POST

</td> </tr>  
<tr>  
<td>

url

</td>  
<td>

记录请求的URL地址

</td>  
<td>

string

</td> </tr>  
<tr>  
<td>

files

</td>  
<td>

记录请求上传的文件

</td>  
<td>

\*

</td> </tr> </table>

## 示例

  * 获取上传的图片并保存到本地

{%ace edit=true, lang='python'%}

    @app.route('/', methods=['POST'])
    def index():
        pic = request.files.get('pic')
        pic.save('./static/aaa.png')
        return 'index'
    
{%endace%}

____

