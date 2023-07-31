## Basic Nginx Manipulation on CentOS
```bash
# login to your remote vps
ssh -i "first-aws-ec2.pem" ec2-user@ec2-35-172-190-158.compute-1.amazonaws.com

# check ip / network card
ifconfig
ip addr

# shutdown
init 0

# Amazon Linux versions
Amazon Linux 2023, Amazon Linux 2, and the Amazon Linux AMI.

# identify your Amazon Linux version, eg: image_name, image_version, image_arch etc info.
cat /etc/image-id

# current release, eg: Amazon Linux release 2023 (Amazon Linux)
cat /etc/system-release

# current os release
cat /etc/os-release

#======================================
# Installing Nginx on Amazon Linux 2023
#======================================
# DNF is a software package manager that installs, updates, and removes packages on Fedora and is the successor to YUM(Yellow-Dog Updater Modified)
# after successfully intalled nginx, we can check config at: /etc/nginx/nginx.conf
1. update your package repository
sudo dnf update -y

2. install nginx
sudo dnf install nginx -y

# systemctl is used to examine and control the state of "systemd" system and service manager.

# systemd is system and service manager for most Unix like operating systems
# As the system boots up, the first process created, i.e. init process with 
# PID = 1, is systemd system that initiates the userspace services.
3. start nginx with systemctl
sudo systemctl start nginx

4. verify nginx installation
sudo systemctl status nginx

5. enable nginx at boots up (if needed)
sudo systemctl enable nginx

# most commonly used nginx commands (need to switch to root role first, eg: sudo su -)
start: nginx
stop: nginx -s stop
quit (stop gently): nginx -s quit
reload (reload config): nginx -s reload

# systemctl version
start: systemctl start nginx
stop: systemctl stop nginx
quit (stop gently): systemctl quit nginx
reload (reload config): systemctl reload nginx
check status: systemctl status nginx
start nginx at boots: systemctl enable nginx

# check amazon linux public ip
dig +short myip.opendns.com @resolver1.opendns.com
or
curl http://checkip.amazonaws.com

# after service is up, then we can access ip address to see server up information
# and with `master process`, and `worker process` started
eg: http://35.172.190.158/

# nginx service script
vi /usr/lib/systemd/system/nginx.service

# restart the machine (do not use in production)
reboot

# unzip and open a gz file using
gunzip archive.gz
```

# Links
Amazon Linux 2023: https://docs.aws.amazon.com/linux/al2023/ug/managing-repos-os-updates.html

Nginx Documentation: http://nginx.org/en/docs/
