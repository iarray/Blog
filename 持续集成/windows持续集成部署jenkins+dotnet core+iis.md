1. 下载安装 https://jenkins.io/zh/download/
2. 安装dotnet core sdk
3. 安装jenkins, 打开浏览器访问 http://localhost:8080
4. 根据提示指引安装jenkins
5. 安装完成后,新建一个Item
6. 选择Freestyle project, 输入项目名
7. 配置git仓库地址
8. 进入项目, 点击配置, 在构建一栏,点击增加构建步骤
9. 选择Execute Windows batch command, 相当于运行cmd执行命令
10. 输入一下内容
```shell
C:\Windows\System32\inetsrv\appcmd.exe stop apppool /apppool.name:"IIS网站名称"
cd ./Sesame
dotnet build
dotnet publish -o "C:\Sesame"
C:\Windows\System32\inetsrv\appcmd.exe start apppool /apppool.name:"IIS网站名称"
```

第一句是停止iis网站,防止程序在运行导致发布失败
最后一句是启动iis网站
