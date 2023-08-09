## URLRewrite and Load Balancer

- Basic Nginx Architecture

<img width="904" alt="basic-nginx-architecture" src="https://github.com/tjcchen/nginx-best-practice/assets/6133656/4e9dc32a-090a-4bd5-be27-715c9b619a00">


### Firewall Commands

- Restrict internet access and only allow specific intranet IP access

```bash
# current IP: 54.208.140.76

# install firewall on Amazon Linux 2023
yum install firewalld -y

# start the firewall ( after the firewall is started, reverse proxy to this machine cannot be accessed )
systemctl start firewalld

# restart the firewall
systemctl restart firewalld

# reload regulations
firewall-cmd --reload

# check regulations
firewall-cmd --list-all
eg:
public
  target: default
  icmp-block-inversion: no
  interfaces:
  sources:
  services: dhcpv6-client mdns ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
	  rule family="ipv4" source address="35.172.190.158" port port="80" protocol="tcp" accept

# add rule: only specific port and ip can access the current machine( after configuration, firewall reload is needed )
firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="35.172.190.158" port protocol="tcp" port="80" accept"
```

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
```
