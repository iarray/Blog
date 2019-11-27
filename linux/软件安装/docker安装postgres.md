1. 下载镜像

   ```
   docker pull post
   ```

   

2. 启动容器

   ```
   docker run --name postgres -e POSTGRES_PASSWORD=123456 --restart=always -d postgres
   ```

   