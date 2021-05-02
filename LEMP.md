#WEB LEMP IMPLEMENTATION

#install ngnix web server

apt update
![image](https://user-images.githubusercontent.com/49937302/114253859-be559000-99de-11eb-8bd8-52ce24599351.png)
apt install nginx -y
![image](https://user-images.githubusercontent.com/49937302/114253948-4b98e480-99df-11eb-9865-ff91019b1aa3.png)
systemctl status ngnix
![image](https://user-images.githubusercontent.com/49937302/114254018-a4687d00-99df-11eb-9079-df298f4a504d.png)
Allow port 80 on security group
![image](https://user-images.githubusercontent.com/49937302/114254123-32dcfe80-99e0-11eb-817c-03378e08af0d.png)
#verify web is reponding locally and from internet
curl http://localhost:80
![image](https://user-images.githubusercontent.com/49937302/114254193-79325d80-99e0-11eb-9cb6-1e7de6a9d9b8.png)
![image](https://user-images.githubusercontent.com/49937302/114254278-f5c53c00-99e0-11eb-9aed-c6a15387bdcc.png)

#install mysql
apt install mysql-server -y
![image](https://user-images.githubusercontent.com/49937302/114254433-eb577200-99e1-11eb-9c34-1f979ce19f0d.png)
mysql_secure_installation
![image](https://user-images.githubusercontent.com/49937302/114254490-383b4880-99e2-11eb-8b6e-c7af2305e540.png)
test login
![image](https://user-images.githubusercontent.com/49937302/114254530-8b150000-99e2-11eb-81eb-d93fa2d37071.png)

#install php

#Install PHP fastCGI process manager & communnicate with mysql database
apt install php-fpm php-mysql -y
![image](https://user-images.githubusercontent.com/49937302/114254632-1db59f00-99e3-11eb-8b90-18dba8017212.png)

#configure PHP
#create new directory for my website & assign owner 
mkdir /var/www/projectweblemp
chown -R $USER:$USER /var/www/projectweblemp
![image](https://user-images.githubusercontent.com/49937302/114254739-b5b38880-99e3-11eb-917f-782cc4e6321e.png)
#create new configuration file
nano /etc/nginx/sites-available/projectweblemp
#/etc/nginx/sites-available/projectweblemp

server {
    listen 80;
    server_name projectweblemp www.projectweblemp;
    root /var/www/projectweblemp;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
![image](https://user-images.githubusercontent.com/49937302/114254902-c0bae880-99e4-11eb-8c85-25f62b719999.png)

ln -s /etc/nginx/sites-available/projectweblemp /etc/nginx/sites-enabled/
nginx -t
![image](https://user-images.githubusercontent.com/49937302/114255809-4f7d3480-99e8-11eb-9e15-c37610991988.png)

unlink /etc/nginx/sites-enabled/default
systemctl reload nginx
systemctl status nginx
![image](https://user-images.githubusercontent.com/49937302/114255863-b3076200-99e8-11eb-95d3-1b846fb1e0b0.png)

echo 'Hello WEBLEMP from projectweblemp' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectweblemp/index.html

![image](https://user-images.githubusercontent.com/49937302/114255936-3a54d580-99e9-11eb-9aea-6ccdccf669f9.png)

![image](https://user-images.githubusercontent.com/49937302/114255954-52c4f000-99e9-11eb-8c3d-fe6553cdb865.png)

#Testing PHP with nginx

nano /var/www/projectweblemp/info.php
![image](https://user-images.githubusercontent.com/49937302/114255992-b0f1d300-99e9-11eb-9784-28f26ef82c01.png)

![image](https://user-images.githubusercontent.com/49937302/114255983-97508b80-99e9-11eb-9346-ffafff13a3b0.png)
rm /var/www/projectweblemp/info.php

#retrieve data from mysql using php

#create db & users
![image](https://user-images.githubusercontent.com/49937302/114256825-a84fcb80-99ee-11eb-862f-9a12967b087c.png)
#verify user can login and access to the database
![image](https://user-images.githubusercontent.com/49937302/114256836-bef62280-99ee-11eb-9f83-3814ff1f4135.png)
#create table and insert content to table
![image](https://user-images.githubusercontent.com/49937302/114257054-d97ccb80-99ef-11eb-8182-525d75666fe2.png)

#create php script and connect to weblempdb
![image](https://user-images.githubusercontent.com/49937302/114257192-adae1580-99f0-11eb-8a0b-3dd815ea0b3b.png)

![image](https://user-images.githubusercontent.com/49937302/114257152-953dfb00-99f0-11eb-848d-a9b0722534fc.png)
![image](https://user-images.githubusercontent.com/49937302/114257169-9cfd9f80-99f0-11eb-8f97-f5fc073d4d38.png)
