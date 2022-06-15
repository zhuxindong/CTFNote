# 题目
![image](https://user-images.githubusercontent.com/24989246/173817143-75fa291a-d14e-4a97-8a8e-a6fa9ec8a00f.png)

# 解题思路
考察的是文件包含，page中带有php://的都会被替换成空，str_replace()以其他字符替换字符串中的一些字符(区分大小写)，strstr() 查找字符串首次出现的位置。返回字符串剩余部分。
```python
<?php
show_source(__FILE__);
echo $_GET['hello'];
$page=$_GET['page'];
while (strstr($page, "php://")) {
    $page=str_replace("php://", "", $page);
}
include($page);
?>
```
程序过滤掉了page=参数传入php://
可以大小写绕过，strstr()，该函数是区分大小写的。不区分大小写的搜索，用 stristr() 函数。。str_replace() 函数替换字符串中的一些字符（区分大小写）。
所以我们构造 ?page=PHP://input
php://input 可以访问请求的原始数据的只读流, 将post请求中的数据作为PHP代码执行。
![image](https://user-images.githubusercontent.com/24989246/173817566-52277ee1-7826-4a58-bf0e-774500199832.png)
很明显，就cat一下就行了
![image](https://user-images.githubusercontent.com/24989246/173817657-33d3f676-ea0c-4ce6-96d0-6d4d74df91e1.png)

