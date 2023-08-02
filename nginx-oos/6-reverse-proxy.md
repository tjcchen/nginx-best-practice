## Nginx Reverse Proxy

### Concepts

- Reverse Proxy
  - A proxy server is a go‑between or intermediary server that forwards requests for content from multiple clients to different servers across the Internet. A reverse proxy server is a type of proxy server that typically sits behind the firewall in a private network and directs client requests to the appropriate backend server. A reverse proxy provides an additional level of abstraction and control to ensure the smooth flow of network traffic between clients and servers.
 
- Diagram

<img width="768" alt="reverse-proxy" src="https://github.com/tjcchen/nginx-best-practice/assets/6133656/b3036472-28f6-498c-9598-e7259bfbe986">


<img width="993" alt="be-server-cluster" src="https://github.com/tjcchen/nginx-best-practice/assets/6133656/7adf1a42-45d0-4cb4-9b2d-21949e642980">


### Common Uses For A Reverse Proxy Server

- Load Balancing
  - A reverse proxy server can act as a “traffic cop,” sitting in front of your backend servers and distributing client requests across a group of servers in a manner that maximizes speed and capacity utilization while ensuring no one server is overloaded, which can degrade performance. If a server goes down, the load balancer redirects traffic to the remaining online servers.

- Web Acceleration
  - Reverse proxies can compress inbound and outbound data, as well as cache commonly requested content, both of which speed up the flow of traffic between clients and servers. They can also perform additional tasks such as SSL encryption to take load off of your web servers, thereby boosting their performance.

- Security
  - By intercepting requests headed for your backend servers, a reverse proxy server protects their identities and acts as an additional defense against security attacks. It also ensures that multiple servers can be accessed from a single record locator or URL regardless of the structure of your local area network.

### Nginx Tips

- Url Rewrite
  - convert a service endpoint to regular url address
  - eg: /item/100 -> /itemService?id=100
  - eg: /item/100.html, friendly to SEO


### Example Config of Reverse Proxy
```conf
# server proxy_pass
server {
    listen       80;
    listen       [::]:80;
    server_name  www.tjcchen.org;
    # root         /www/www;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
      # note: `proxy_pass` and `root` cannot be coexisted
      # https redirect is not yet supported
      # proxy_pass http://www.tjcchen.com;

      # reverse proxy for current host (35.172.190.158) to another host (54.90.189.4)
      # http://35.172.190.158/?abc=123 will show content of http://54.90.189.4
      proxy_pass http://54.90.189.4;

      # index index.html index.htm;
    }

    error_page 404 /404.html;
    location = /404.html {
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
    }
}
```

### Sub Servers Info
```bash
# serv1
ssh -i "first-aws-ec2.pem" ec2-user@ec2-54-90-189-4.compute-1.amazonaws.com


```

