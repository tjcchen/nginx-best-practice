## Nginx Directory Structure
```bash
# login to your remote vps
ssh -i "first-aws-ec2.pem" ec2-user@ec2-35-172-190-158.compute-1.amazonaws.com

# list all the nginx config
# etc dir means: Editable Text Configuration
# eg: /etc/nginx/
ll /etc/nginx/

# main nginx config (we can split nginx.conf to multiple modules)
cat /etc/nginx/nginx.conf

# current public ip:
http://35.172.190.158/index3.html

##################################################
# default html pages dir
# find this folder path from server/root directive
# eg: /usr/share/nginx/html
##################################################
cd /usr/share/nginx/html

#######################################################
# default logs dir
# we have access.log and error.log included in this dir
# eg: /var/log/nginx
#######################################################
cd /var/log/nginx

# check access log
tail -n 10 access.log

# check error log
tail -n 10 error.log

#########################################
# execute the nginx main process program
# eg: /usr/sbin/nginx
#########################################
program: /usr/sbin/nginx
execute (start the nginx server): cd /usr/sbin && ./nginx
```