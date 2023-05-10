## 1. 构建压缩[#](https://quliangen.github.io/blog/project/cicd/deploy#_1-%E6%9E%84%E5%BB%BA%E5%8E%8B%E7%BC%A9)

bash

```
# 构建
pnpm run build
# 压缩产物
tar -czvf dist.tar.gz dist/
```

## 2. 登录并上传资源到服务[#](https://quliangen.github.io/blog/project/cicd/deploy#_2-%E7%99%BB%E5%BD%95%E5%B9%B6%E4%B8%8A%E4%BC%A0%E8%B5%84%E6%BA%90%E5%88%B0%E6%9C%8D%E5%8A%A1)

shell

```
# 上传 静态资源。xxx替换为实际服务地址，根据实际路径定义。
scp dist.tar.gz root@xx.xxx.xxx.xxx:/data/web/tmp
# 提示输入服务密码，输入即可。
```

## 3. 登录服务替换资源[#](https://quliangen.github.io/blog/project/cicd/deploy#_3-%E7%99%BB%E5%BD%95%E6%9C%8D%E5%8A%A1%E6%9B%BF%E6%8D%A2%E8%B5%84%E6%BA%90)

shell

```
# 终端登录服务。xxx替换为实际服务地址。
ssh root@xx.xxx.xxx.xxx
# 解压资源包，删除当前文件夹中已有的 dist目录，解压二上传的资源。
cd /data/web/temp; rm -rf dist; tar -xzvf dist.tar.gz
# 删除部署的目录内的文件夹，新的资源移动到部署目录。
rm -rf /data/web/wap/static; mv /data/web/tmp/dist/* /data/web/wap
# 重启 NG 可选，修改配置需重启。
# /usr/local/openresty/nginx/sbin/nginx -s reload
```

## 4：增加发布 参照 [前端增量部署](https://quliangen.github.io/blog/project/cicd/incremental)