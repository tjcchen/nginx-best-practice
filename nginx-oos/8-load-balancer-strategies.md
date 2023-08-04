## Load Balancer Strategies

- weight (forward requests as per machine & network performances)
- down (make the current server down)
- backup (a backup machine - when other servers are all down, we will use this backup machine)

less used:
- ip_hash (keep session connection based on IP address)
- least_conn (number of least connections)
- fair (allocate requests based on server response time, 3rd party plugin needed)
- url_hash (redirect request based on users' url, 3rd party plugin needed)

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
    # weight - the higher the weight, the more traffic the machine will receive
    # down - make the server down
    # backup - a backup machine
    server 54.90.189.4 weight=8 down;
    server 54.208.140.76 weight=2;
    server 3.89.65.247 weight=1 backup;
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

### Disadvantages of Round-robin Load Balancing

- Can't keep session connection on any machines (eg: login flow)

### How to keep session connection in an microservice architecture

- Spring Session (store session in one machine with redis)

- Auth server, token
