1. 下载镜像

```jsx
docker pull jenkins
```

2. 运行

   ```jsx
   cd ~
   mkdir jenkins
   sudo chmod 777 -R ~/jenkins
   
   docker run -d -p 8686:8080 -p 50000:50000 -v ~/jenkins:/var/jenkins_home --name jenkins --restart=always jenkins
   ```

3. 配置

   ```
   docker logs jenkins
   ```

   查看jenkins日志, 可以看到类似下面的信息

   ```
   
   *************************************************************
   *************************************************************
   *************************************************************
   
   Jenkins initial setup is required. An admin user has been created and a password generated.
   Please use the following password to proceed to installation:
   
   0ae151ee870c4d9d8dffb3d3e270c96f
   
   This may also be found at: /var/jenkins_home/secrets/initialAdminPassword
   
   *************************************************************
   *************************************************************
   *************************************************************
   
   ```

   启动浏览器, 访问http://ip:8686 根据提示把 0ae151ee870c4d9d8dffb3d3e270c96f 复制出来粘贴上去