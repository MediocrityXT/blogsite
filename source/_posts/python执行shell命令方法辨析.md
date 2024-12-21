---
title: python执行shell命令方法辨析
date: '2024-12-12 20:40:03'
math: false
excerpt: ''
tags:
- linux
- shell
---

# 两种方法: os.popen,  subprocess.Popen
1. output = os.popen('cmd')
	1. 只有stdout会返回到output，使用output.read()读取
	2. cmd的stderr不会返回，而是直接重定向到用户终端stdout
	3. output.read()获取stdout的文本内容会阻塞python脚本，直到cmd完全执行完毕并return code，并且返回output。参考[output.read()阻塞测试]( #output .read()阻塞测试)
2. process = subprocess.Popen('cmd', **shell=True**, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
	1. shell=True是以bash命令方式执行cmd，否则是读取脚本名
	2. stdout=subprocess.PIPE, stderr=subprocess.PIPE指定了cmd输出到PIPE
	3. output, error = process.communicate()返回命令执行完毕后的subprocess.PIPE的stdout和stderr流
	4. 同理process.communicate()默认阻塞python脚本，等待cmd完全执行完毕返回return code。参考[process.communicate()阻塞测试]( #process .communicate()阻塞测试)

# output.read()阻塞测试
```
print(time.time())
a = os.popen('sleep 2 && echo returned && sleep 5')
print(time.time())
print(a.read())
print(time.time())

---
1734007374.634668
1734007374.646317
returned

1734007381.65101
```

# process.communicate()阻塞测试
```
print(time.time())
a = subprocess.Popen('sleep 2 && echo returned && sleep 5',shell=True)
print(time.time())
print(a.communicate())
print(time.time())

---
1734008458.643937
1734008458.6443274
returned
(None, None)
1734008465.6490426


# 加上stdout和stderr参数
print(time.time())
a = subprocess.Popen('sleep 2 && echo returned && sleep 5',shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
print(time.time())
print(a.communicate())
print(time.time())

---
1734009087.784299
1734009087.7847497
(b'returned\n', b'')
1734009094.7897353
```

# 参考资料
[python popen 怎么得到结果-CSDN博客](https://blog.csdn.net/weixin_32271623/article/details/140745306)