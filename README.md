# EC2 ubuntu environment setup 
 1. sudo apt update 
 2. sudo add-apt-repository ppa:ondrej/php   
 3. curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -   
 4. sudo apt-get install -y nodejs  
 5. sudo apt install zip unzip 
 6. sudo apt install nginx  
 7. sudo apt update

# GIT install
  1. sudo apt-get install git

# Install mysql:
 1. ```sudo apt install mysql-server```

# PHP 8.0 fpm install:
 1. ```sudo apt install php8.0-fpm php8.0-mysql```  
 2. ```sudo apt install php8.0-mbstring php8.0-xml php8.0-bcmath php8.0-simplexml php8.0-intl php8.0-mbstring php8.0-gd php8.0-curl php8.0-zip ```
 
# PHP 7.4 fpm install:
 1. ```sudo apt install php7.4-fpm php7.4-mysql```  
 2. ```sudo apt install php7.4-mbstring php7.4-xml php7.4-bcmath php7.4-simplexml php7.4-intl php7.4-mbstring php7.4-gd php7.4-curl php7.4-zip ```

# composer install:
 1. ```php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"``` 
 2. ```php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"``` 
3. ```php composer-setup.php``` 
4. ```php -r "unlink('composer-setup.php');"``` 
5. ```sudo mv composer.phar /usr/bin/composer``` 

# Create Database and user(login to mysql server)
 
    1. sudo mysql
    2. CREATE DATABASE school_managment_new;
    3. CREATE USER 'school_user'@'%' IDENTIFIED WITH mysql_native_password BY 'school_user';
    5. GRANT ALL ON school_managment_new.* TO 'school_user'@'%';
    6. FLUSH PRIVILEGES;

# Edit example sites-available file 

 ```sudo vim /etc/nginx/sites-available/example```

# Ninx file configure:
```
server {
    listen 80;
    server_name server_domain_or_IP;
    root /var/www/example/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock; // set you fpm path
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

# Setting Up Nginx

 1. ```sudo ln -s /etc/nginx/sites-available/example /etc/nginx/sites-enabled/```

# check nginx have any error and reload
  1. ```sudo nginx -t```
  2. ```sudo systemctl reload nginx```
  
  
# Set permission

 1. ```sudo chown -R www-data.www-data /var/www/project/storage```
 2. ```sudo chown -R www-data.www-data /var/www/project/bootstrap/cache```
