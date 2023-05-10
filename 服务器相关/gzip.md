Nginx 可以通过 gzip 压缩减小传输的流量，加快网站的访问速度。下面是 Nginx 开启 gzip 的配置方法：

1.  打开 Nginx 配置文件：
    
    ```
    复制$ sudo nano /etc/nginx/nginx.conf
    ```
    
2.  搜索 `gzip` 配置项：
    
    ```
    复制# gzip types
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
    ```
    
    在 `http` 块中可以找到 `gzip` 配置项，将 `gzip` 设置为 `on`，并设置 `gzip_types` 将要压缩的文件类型。
    
3.  配置 `gzip_comp_level`：
    
    ```
    复制gzip_comp_level 6;
    ```
    
    `gzip_comp_level` 表示压缩等级，值范围从 1 到 9，值越大压缩的越充分，但同时也会消耗更多的 CPU 资源。
    
4.  配置 `gzip_min_length`：
    
    ```
    复制gzip_min_length 1000;
    ```
    
    `gzip_min_length` 表示只对压缩后长度大于等于 1000 字节的文件进行压缩。
    
5.  配置 `gzip_buffers`：
    
    ```
    复制gzip_buffers 16 8k;
    ```
    
    `gzip_buffers` 表示压缩缓冲区的大小。
    
6.  重启 Nginx 服务：
    
    ```
    复制$ sudo systemctl restart nginx
    ```
    

这样就完成了 Nginx 配置 gzip 压缩的操作，下次访问网站时，Nginx 会将满足条件的文件进行压缩并传输。