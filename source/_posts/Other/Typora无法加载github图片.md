# Typora无法加载github图片

使用github作为图床，本地使用typora预览文件，图片无法打开。

这是github图片被墙了，修改`host`文件：

<!--more-->

文件路径：`C:\Windows\System32\drivers\etc`

添加配置:

```shell
# GitHub Start 
192.30.253.112    github.com  // 这里不需要，删除，不然等会打不开Github
192.30.253.119    gist.github.com
151.101.184.133    assets-cdn.github.com
151.101.184.133    raw.githubusercontent.com
151.101.184.133    gist.githubusercontent.com
151.101.184.133    cloud.githubusercontent.com
151.101.184.133    camo.githubusercontent.com
151.101.184.133    avatars0.githubusercontent.com
151.101.184.133    avatars1.githubusercontent.com
151.101.184.133    avatars2.githubusercontent.com
151.101.184.133    avatars3.githubusercontent.com
151.101.184.133    avatars4.githubusercontent.com
151.101.184.133    avatars5.githubusercontent.com
151.101.184.133    avatars6.githubusercontent.com
151.101.184.133    avatars7.githubusercontent.com
151.101.184.133    avatars8.githubusercontent.com
 # GitHub End
```



