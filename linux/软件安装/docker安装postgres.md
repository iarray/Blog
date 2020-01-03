1. 下载镜像

   ```
   docker pull post
   ```

   

2. 启动容器

   ```
   docker run --name postgres -p 5432:5432 -e POSTGRES_PASSWORD=123456 --restart=always -d postgres
   ```

   