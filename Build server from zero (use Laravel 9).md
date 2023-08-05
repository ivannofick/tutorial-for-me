1. Download the system operation ISO from [Url download](https://releases.ubuntu.com/), im using [Ubuntu 20.04](https://releases.ubuntu.com/20.04) 
2. First update system linux using command `sudo apt update` on terminal
3. After update system linux, now execute command using `sudo apt-get install net-tools` for installing tools network(examp `ifconfig`)
> if use virtual machine(i'm using VirtualBox) im prefered remote vm using ssh, please following this tutorial
    > 1. install ssh server using command `sudo apt install openssh-server`
    > 2. change default setting to `setting -> network -> select on tab adapter 1 -> change attached to  Bridge Adapter (default is NAT)
5. Install package php, run ``sudo add-apt-repository ppa:ondrej/php -y``, after that run the command if u want use php version 7.4 ``sudo apt install php7.4-cli php7.4-fpm php7.4-common php7.4-mbstring php7.4-curl php7.4-xml php7.4-bcmath -y`` if you want use php8.1 run command ``sudo apt install php8.1-cli php8.1-fpm php8.1-common php8.1-mbstring php8.1-curl php8.1-xml php8.1-bcmath -y`` but if you want to install two version then explain both command alternately and after installing both version but you want to use one of them then run command ``sudo update-alternatives --config php `` after that select one version using numbers.
6. After install php, install nginx using command `` sudo apt install nginx``
7. After install nginx, install openssl for ssl website run command ``sudo apt install opensll`` if there is an error like
    > Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    E: Unable to locate package opensll
    
    then check openssl using command ``openssl version``, after that install openssl now generate ssl using command 
    > 1. ``sudo mkdir /etc/nginx/certificate``
    > 2. ``cd /etc/nginx/certificate``
    > 3. ``sudo openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out nginx-certificate.crt -keyout nginx.key`` and insert value like below 
    ```
    Generating a RSA private key
    ..............++++
    ....++++
    writing new private key to 'nginx.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:ID
    State or Province Name (full name) [Some-State]:Jawa Tengah
    Locality Name (eg, city) []:Semarang
    Organization Name (eg, company) [Internet Widgits Pty Ltd]:trondolanbalap
    Organizational Unit Name (eg, section) []:balapan
    Common Name (e.g. server FQDN or YOUR name) []:ynkts
    Email Address []:trondolan.satriafu
    ```

8. After install openssl, now install composer using command, follow the tutorials bellow
    > 1. curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
    > 2. HASH='curl -sS https://composer.github.io/installer.sig'
    > 3. php -r "if (hash_file('SHA384', '/tmp/composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
    > 4. sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer
    > 5. composer -v

9. Create a folder to put the application in ``/var/www``, follow step below
    > 1. run command ``cd /var/www/``
    > 2. run command ``sudo mkdir application``
    > 3. run command ``cd application``
    > 4. run command ``sudo chmod 775 -R .``
10. Now install [laravel](https://laravel.com/docs/9.x) run command ``composer create-project laravel/laravel:^9.0`` if any error like a below
    ```
    Creating a "laravel/laravel:^9.0" project at "./"
    Installing laravel/laravel (v9.5.2)
    Failed to download laravel/laravel from dist: The zip extension and unzip/7z commands are both missing, skipping.
    Your command-line PHP is using multiple ini files. Run `php --ini` to show them.
    Now trying to download from source
      - Syncing laravel/laravel (v9.5.2) into cache
      - Installing laravel/laravel (v9.5.2): Cloning 6092ff46b3 from cache
    Install of laravel/laravel failed
    In Filesystem.php line 314:       
      Could not delete /var/www/application/.:  
    ```
    you can run command `` sudo apt install zip unzip php8.1-zip `` if you want install on php7.4 run command ``sudo apt install zip unzip php7.4-zip ``
11. now configuration config nginx, create file using command `` cd /etc/nginx/sites-available/ `` after that run command `` sudo touch application.conf `` and insert syntax like a below
    ```
    # Configure sites-avaliable nginx using php8.1 and SSL

    server {
        listen 80;
        listen [::]:80;

        server_name application.com;

        # Redirect all HTTP requests to HTTPS with a 301 Moved Permanently response.
        return 301 https://$host$request_uri;
    }

    server {
        listen 443 ssl default_server;
        listen [::]:443 ssl default_server;

        root /var/www/application/public;
        index index.php index.html index.htm;

        server_name application.com;

        #ssl on;
        ssl_certificate /etc/nginx/certificate/nginx-certificate.crt;
        ssl_certificate_key /etc/nginx/certificate/nginx.key;

        add_header Strict-Transport-Security max-age=15768000;

        location / {
            try_files $uri $uri/ /index.php?$query_string;
        }


        location ~ \.php$ {
            fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
            include fastcgi_params;
        }

    }
    ```
12. after make configuration nginx, run command follow tutorial on below
    > 1. `` sudo ln -s /etc/nginx/sites-available/application.conf  /etc/nginx/sites-enabled/ ``
    > 2. make sure after running ``sudo nginx -t`` it make sure something like below
    ```
        nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
        nginx: configuration file /etc/nginx/nginx.conf test is successful
    ```
    > 3. ``sudo systemctl restart nginx``
    but when you open your application and the browser displays the apache settings, then follow the steps below
    ```
        1. sudo systemctl stop apache2
        2. sudo systemctl disable apache2
        3. sudo apt remove apache2
        4. sudo apt purge apache2
    ```

13. Next setup is about [installing docker](https://github.com/ivannofick/tutorial-for-me/blob/main/Install%20Docker%20in%20ubuntu.md)