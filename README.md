# nginx
An official read-only mirror of http://hg.nginx.org/nginx/ which is updated hourly. Pull requests on GitHub cannot be accepted and will be automatically closed. The proper way to submit changes to nginx is via the nginx development mailing list, see http://nginx.org/en/docs/contributing_changes.html

wget http://soft.vpser.net/lnmp/lnmp1.8.tar.gz -cO lnmp1.8.tar.gz && tar zxf lnmp1.8.tar.gz && cd lnmp1.8 
##if you want to install nginx only, use bellow
wget http://soft.vpser.net/lnmp/lnmp1.8.tar.gz -cO lnmp1.8.tar.gz && tar zxf lnmp1.8.tar.gz && cd lnmp1.8 && ./install.sh lnmp nginx

##setup a reverse proxy
upstream xys {
        server www.xys.org;
}
server {
        listen 80;
        #listen [::]:80;
        server_name a.yxwmed.com;

        location / {
            proxy_pass http://xys;
            }
            }
##不带证书的普通反代			
server
	{
    	listen          80;
    	server_name     www.kwxonline.com;

    	location / {
        proxy_pass          http://www.v5.net/;
        proxy_redirect      off;
        proxy_set_header    X-Real-IP       $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
    		}
	}

##带证书的SSL配置
##如果安装过bt，则配置文件在：/www/server/panel/vhost/nginx/privkey.pem
server
{
    listen 80;
        listen 443 ssl http2;
    server_name s.genome.ml;
    index index.php index.html index.htm default.php default.htm default.html;

    #SSL-START SSL相关配置，请勿删除或修改下一行带注释的404规则
    #error_page 404/404.html;
    ssl_certificate    /usr/local/nginx/conf/vhost/fullchain.pem;
    ssl_certificate_key    /usr/local/nginx/conf/vhost/privkey.pem; 
    ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    error_page 497  https://$host$request_uri;

    #SSL-END
location / {
        proxy_pass          https://scholar.google.com/;
        proxy_redirect      off;
        proxy_set_header    X-Real-IP       $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        }
}
##重启nginx
service nginx start/reload
##测试nginx的conf文件是否OK
/usr/bin/nginx -t

##如果需要发送域名的，则用以下语句。
location /
{
    expires 12h;
    if ($request_uri ~* "(php|jsp|cgi|asp|aspx)")
    {
         expires 0;
    }
    proxy_pass https://zh.wikipedia.org;
    proxy_set_header Host zh.wikipedia.org;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    add_header X-Cache $upstream_cache_status;
}
}	
  
