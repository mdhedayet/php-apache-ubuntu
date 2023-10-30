Running different PHP versions with Apache on Ubuntu can be achieved using the `php-fpm` package along with Apache's `proxy_fcgi` module. Here's a step-by-step guide to help you set this up:

### Step 1: Install Apache and PHP
Make sure you have Apache and the required PHP versions installed on your Ubuntu system. You can install different PHP versions using the `ondrej/php` PPA repository. Here's an example of how to install PHP 7.4 and PHP 8.0:

```bash
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt-get install -y apache2
sudo apt-get install -y php7.4 php7.4-fpm
sudo apt-get install -y php8.0 php8.0-fpm
```

### Step 2: Enable necessary Apache modules
```bash
sudo a2enmod proxy_fcgi setenvif
sudo a2enconf php7.4-fpm
sudo a2enconf php8.0-fpm
sudo service apache2 restart
```

### Step 3: Configure Apache Virtual Hosts
You can set up different Virtual Hosts for different PHP versions. An example configuration file (`/etc/apache2/sites-available/example.com.conf`) for a Virtual Host might look like this:

```apache
<VirtualHost *:80>
    ServerName example.com
    DocumentRoot /var/www/example.com

    <FilesMatch \.php$>
        SetHandler "proxy:unix:/run/php/php7.4-fpm.sock|fcgi://localhost"
    </FilesMatch>

    <Directory /var/www/example.com>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

<VirtualHost *:80>
    ServerName example2.com
    DocumentRoot /var/www/example2.com

    <FilesMatch \.php$>
        SetHandler "proxy:unix:/run/php/php8.0-fpm.sock|fcgi://localhost"
    </FilesMatch>

    <Directory /var/www/example2.com>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

### Step 4: Restart Apache
After making changes to the Apache configuration, don't forget to restart Apache for the changes to take effect:

```bash
sudo service apache2 restart
```

With these steps, you should be able to run different PHP versions with Apache on your Ubuntu system. Adjust the configurations according to your specific setup and requirements.
