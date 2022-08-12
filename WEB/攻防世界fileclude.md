# 1.题目描述
![image](https://user-images.githubusercontent.com/24989246/184316645-654fb2b8-6ef1-465e-9a79-7b0d982ca5e1.png)

# 2.解题思路
## 2.1
启动场景可知，flag应该是在flag.php里面，只要能执行include($file1);这一行，就能利用伪协议"php://filter/read=convert.base64-encode/resource=./flag.php"
来读取flag.php的源代码。
## 2.2
阅读代码发现，能执行到这一行的前提是file_get_contents($file2) === "hello ctf"。也就是file2文件内容是"hell ctf"
一开始直接尝试传入参数file2=hello ctf，
![image](https://user-images.githubusercontent.com/24989246/184317269-a3298523-e717-41e3-a021-718f73f04d45.png)
但发现报错，原因是file_get_contents()这个函数需要传入文件路径，我们没上传文件，所以报错找不到文件。
## 2.3
想办法绕开，网上看了一下：
file_get_contents()的$filename参数不仅仅为本地文件路径，还可以是一个网络路径URL，
所以利用伪协议data://text/plain;base64,aGVsbG8gY3Rm  （aGVsbG8gY3Rm经过base64解码之后是hello ctf）。
![image](https://user-images.githubusercontent.com/24989246/184318035-62ab2215-4381-4cd5-8e17-26621961e12e.png)
成功得到一个base64编码的字符，解码之后就是flag了
![image](https://user-images.githubusercontent.com/24989246/184318109-0cbce55e-52b3-46b2-ac66-c80ef1fc746b.png)
