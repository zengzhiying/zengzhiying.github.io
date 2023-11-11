# zengzhiying.net 个人主站

当前版本: 1.0.6

纯静态页面，可以链接到其他常用的网站。

## 1.配置注意

1. 需配置 `www.zengzhiying.net` 重定向至 `zengzhiying.net`
2. 提前解析域名 A 记录到服务器，分别为 `@` 和 `www`
3. 当前安装位置为：`/opt/zengzhiying.net`



## 2.解压静态页面

克隆仓库（需要有 SSH 密钥）或解压静态页面到服务器：

```shell
# 如果网络问题无法克隆，直接本地打包上传即可
cd /opt
git clone git@github.com:monchickey/zengzhiying.net.git
# 注意删除 Markdown 和 Git 相关的文件
cd zengzhiying.net
rm -f README.md RELEASE.md
rm -rf .git
```



## 3.配置 nginx

创建配置 `/etc/nginx/sites-enabled/zengzhiying.conf` 添加 server 块配置如下：

```nginx
server {
    listen 80;
    server_name www.zengzhiying.net;
    rewrite ^/(.*) http://zengzhiying.net/$1 permanent;
}

server {
    listen       80;
    server_name  zengzhiying.net;

    root /opt/zengzhiying.net;
    index  index.html index.htm;

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /var/www/html;
    }
}
```

配置完成后保存，然后重新载入 nginx 配置：

```shell
nginx -s reload
```

然后访问页面查看。
