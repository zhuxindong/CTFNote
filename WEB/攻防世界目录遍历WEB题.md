# 解题思路：
1.尝试猜常见的备份文件名，如.bak .back
2.用工具遍历服务器目录，比如 https://github.com/maurosoria/dirsearch 工具
执行命令
python dirsearch.py -u http://111.200.241.244:59245/ -e *
得到文件名
![image](https://user-images.githubusercontent.com/24989246/173810047-bb88bb54-2054-4ab2-997f-455c6fb709d1.png)
