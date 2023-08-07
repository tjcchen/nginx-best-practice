## Static and Dynamic Separation

- Use cases
  - small-to-medium size website, embeded H5 page to an App

- Mechanism
  - move your Backend Server(Tomcat) js/css/img resources to the "front door" nginx server

- Diagram

<img width="519" alt="static-dynamic-separation" src="https://github.com/tjcchen/nginx-best-practice/assets/6133656/6b85449b-2b1d-47e8-8a48-d543509ba802">


- Performance Comparison between Tomcat and Nginx

<img width="428" alt="tomcat-static-resources" src="https://github.com/tjcchen/nginx-best-practice/assets/6133656/7d37b05c-48b3-46ee-9f9a-78fcaa70d8ed">



## Static and Dynamic Separation Config
```conf
# css/js/img config separately
server {
    listen       80;
    listen       [::]:80;
    server_name  www.tjcchen.org;

    # static and dynamic separation
    root /usr/share/nginx/html;

    location / {
        # static and dynamic separation
        proxy_pass http://54.208.140.76;

        # load balancer
        # proxy_pass http://servers;

        # reverse proxy
        # proxy_pass http://www.tjcchen.com;

        # default
        # index index.html index.htm;
    }

    # static and dynamic separation
    # css/js/img
    # /css /js /img folders need to be placed under /usr/share/nginx/html
    location /css {
    }

    location /js {
    }

    location /img {
    }

    error_page 404 /404.html;
    location = /404.html {
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
    }
}

# regExp matching for route css/js/img
server {
    listen       80;
    listen       [::]:80;
    server_name  www.tjcchen.org;

    # static and dynamic separation
    root /usr/share/nginx/html;

    location / {
        # static and dynamic separation
        proxy_pass http://54.208.140.76;
    }

    # regExp route matching for css/js/img directory
    # static and dynamic separation
    # css/js/img
    # /css /js /img folders need to be placed under /usr/share/nginx/html

    # ~: use regExp
    # *: case insensitative
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
