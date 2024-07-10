# nginx-config-for-laravel-app

[How To Install and Configure Laravel with Nginx on Ubuntu 22.04 (LEMP)](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-laravel-with-nginx-on-ubuntu-22-04)



------------------------------------------------------------------------------------

`sudo mv ~/site-name /var/www/site-name`      // замість `mv` можна скопіювати `cp`

------------------------------------------------------------------------------------

- `sudo chown -R www-data.www-data /var/www/site-name/storage`
- `sudo chown -R www-data.www-data /var/www/site-name/bootstrap/cache`
- `sudo chown -R www-data:www-data /var/www/site-name/public`

------------------------------------------------------------------------------------

sudo nano /etc/nginx/sites-available/site-name


    server {
    listen 80;
    server_name site-name.test;
    root /var/www/site-name/public;

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
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}


------------------------------------------------------------------------------------

sudo ln -s /etc/nginx/sites-available/site-name /etc/nginx/sites-enabled/

------------------------------------------------------------------------------------


cd /var/www/site-name

sudo npm run build    (or `sudo npm run dev`)

------------------------------------------------------------------------------------



In file /etc/hosts

    
    127.0.0.1   site-name.test
    
------------------------------------------------------------------------------------   
    
sudo nginx -t

------------------------------------------------------------------------------------

sudo systemctl reload nginx

------------------------------------------------------------------------------------



http://site-name.test








