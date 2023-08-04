## Load Balancer Strategies

- weight (forward requests as per machine & network performances)


### Login Config
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

# serv3
ssh -i "first-aws-ec2.pem" ec2-user@ec2-3-89-65-247.compute-1.amazonaws.com
ip: 3.89.65.247
```

### Weight
```conf
upstream servers {
    server 54.90.189.4:80 weight=8;
    server 54.208.140.76:80 weight=2;
    server 3.89.65.247:80 weight=1;
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