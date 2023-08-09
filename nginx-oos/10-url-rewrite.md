## URLRewrite

- Hide backend url addresses

## Example

Convert

- nginx: http://35.172.190.158/index.jsp?pageNum=2
- real tomcat: http://54.208.140.76/index.jsp?pageNum=2

To

- nginx: http://35.172.190.158/2.html


## Config
```conf
# current ip: 35.172.190.158
server {
    listen       80;
    listen       [::]:80;
    server_name  www.tjcchen.org;

    # static and dynamic separation
    root /usr/share/nginx/html;

    location / {
        # rewrite rules
        # rewrite server location flag;

        # eg1: matching http://35.172.190.158/2.html to http://35.172.190.158/index.jsp?pageNum=2
        rewrite ^/2.html$   /index.jsp?pageNum=2   break;
        
        # eg2: matching http://35.172.190.158/[number].html to http://35.172.190.158/index.jsp?pageNum=[number]
        # $1 means the first match regulation
        rewrite ^/([0-9]+).html$   /index.jsp?pageNum=$1   break;

        # eg3: redirect flag will expose the real url address, like http://35.172.190.158/index.jsp?pageNum=123
        rewrite ^/([0-9]+).html$   /index.jsp?pageNum=$1   redirect;

        # a simple custom config, /ab.html -> /about.html
        rewrite ^/ab.html$ /about.html break;


        # rewrite flags
        # last: This flag will stop the processing of the rewrite directives in the current set, and will start at the new location that matches the changed URL( multiple url rewrite regulations ).
        # break: This flag will stop the processing of the rewrite directives in the current set.
        # redirect: This flag will do a temporary redirection using 302 HTTP code. This is mainly used when the replacement string is not http, or https, or $scheme
        # permanent: This flag will do a permanent redirection using 301 HTTP code


        # static and dynamic separation
        proxy_pass http://54.208.140.76;
    }

    # regExp route matching for css/js/img directory
    location ~*/(js|img|css) {
    }

    error_page 404 /404.html;
    location = /404.html {
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
    }
}
```
