## Domain

### Common Records Explanation

A record: mapping a domain to an IP address.

CNAME: mapping domain to other domains, which is equivalent to domain alias.

NS: DNS server analysis

MX( mail exchange ): Email services, eg: SMTP, POP3


### Generic Domain Record

A record: *.tjcchen.org - (whatever.tjcchen.org can assess your website)

Use cases: multiple users application, eg: james.tjcchen.org

A record: tjcchen.org - (with sub domain name left blank)

Use cases: http://tjcchen.org


### Multiple Port Access

To allow multiple ports access, like http://35.172.190.158:8080/ or http://35.172.190.158:8081/,
We need to open port support on host's specific security group.


### One Host/IP supporting multiple domain names

```conf
# 1. adding multiple servers config to your nginx.conf
# domain config for www.tjcchen.org
server {
    listen       80;
    listen       [::]:80;
    server_name  www.tjcchen.org;
    root         /www/www;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
    index index.html index.htm;
    }

    error_page 404 /404.html;
    location = /404.html {
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
    }
}

# domain config for video.tjcchen.org
server {
    listen       80;
    listen       [::]:80;
    server_name  video.tjcchen.org;
    root         /www/video;

    # Load configuration files for the default server block.
    # include /etc/nginx/default.d/*.conf;

    location / {
    index index.html index.htm;
    }

    error_page 404 /404.html;
    location = /404.html {
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
    }
}

# 2. adding A record on your Amazon Route 53
www.tjcchen.org -> 35.172.190.158
video.tjcchen.org -> 35.172.190.158
```

### Multiple Domains In One Server Config
```conf
server {
    listen       80;
    listen       [::]:80;
    # conclusion: we can use server_name config to accept all the http requests,
    # then uses different routing logic to handle it.
    # 1. support video, video1 domains
    # server_name  video.tjcchen.org video1.tjcchen.org;
    # 2. support wilcard matching suffix, like www.tjcchen.com, www.tjcchen.ca
    # server_name  www.tjcchen.*;
    # 3. support regExp matching, domain start with a number
    # server_name ~^[0-9]+\.tjcchen\.org$;
    # 4. support wilcard matching prefix, like example.tjcchen.org. note only one server_name directive could exist
    server_name  *.tjcchen.org;
    root         /www/video;

    # Load configuration files for the default server block.
    # include /etc/nginx/default.d/*.conf;

    location / {
    index index.html index.htm;
    }

    error_page 404 /404.html;
    location = /404.html {
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
    }
}
```