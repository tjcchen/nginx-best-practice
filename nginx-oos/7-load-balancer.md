## Nginx Load Balancer

### Concept

- A load balancer acts as the “traffic cop” sitting in front of your servers and routing client requests across all servers capable of fulfilling those requests in a manner that maximizes speed and capacity utilization and ensures that no one server is overworked, which could degrade performance.

- Simplest Load Balancer Config
```conf
http {
    upstream myapp1 {
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://myapp1;
        }
    }
}
```

### Load Balancer
```bash
# main
ssh -i "first-aws-ec2.pem" ec2-user@ec2-35-172-190-158.compute-1.amazonaws.com
ip: 35.172.190.158

# serv1
ssh -i "first-aws-ec2.pem" ec2-user@ec2-54-90-189-4.compute-1.amazonaws.com
ip: 54.90.189.4

# serv2
ssh -i "first-aws-ec2.pem" ec2-user@ec2-54-208-140-76.compute-1.amazonaws.com
ip: 54.208.140.76
```

```conf
upstream servers {
    server 54.90.189.4:80;
    server 54.208.140.76:80;
}

server {
    listen       80;
    listen       [::]:80;
    server_name  www.tjcchen.org;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    # servers needs to be configured in upstream to support load balancer
    location / {
        proxy_pass http://servers;
    }

    error_page 404 /404.html;
    location = /404.html {
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
    }
}
```