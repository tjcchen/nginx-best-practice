## Configuring the API gateway
1. Edit your nginx config file:

mac dir: /usr/local/etc/nginx/default.conf

linux dir: /etc/nginx/available-sites/default


2. Set one upstream directive for each microservice


3. Use location and proxy_pass directives to create routes for the upstream microservice servers


## Simple Example

```conf
http {
  upstream shipping {
    server 162.243.144.211:8081; # we can add multiple server directive to support load balancer
  }

  upstream reporting {
    server 162.243.144.211:8082;
  }

  server {
    listen 80;

    # the url http://www.example.com/shipping to use upstream shipping server config
    location /shipping/ {
      proxy_pass http://shipping/;
    }

    location /reporting/ {
      proxy_pass http://reporting/;
    }
  }
}
```

