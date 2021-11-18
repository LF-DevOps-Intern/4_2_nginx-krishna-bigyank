Install the `php-fpm` module

```bash
sudo apt install php-fpm php-mysql
```

create a new server block configuration file within the `/etc/nginx/sites-available/` directory and keep the following configuration

```
server {
        listen 80;
        root /var/www/html;
        index index.php index.html index.htm index.nginx-debian.html;
        server_name your_domain;

        location / {
                try_files $uri $uri/ =404;
        }

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        }

        location ~ /\.ht {
                deny all;
        }
}
```

Here’s what each directives and location blocks does:

- `listen` — Defines what port Nginx will listen on. In this case, it will listen on port `80`, the default port for HTTP.
- `root` — Defines the document root where the files served by the website are stored.
- `index` — Configures Nginx to prioritize serving files named `index.php` when an index file is requested if they’re available.
- `server_name` — Defines which server block should be used for a given request to your server. **Point this directive to your server’s domain name or public IP address.**
- `location /` — The first location block includes a `try_files` directive, which checks for the existence of files matching a URI request. If Nginx cannot find the appropriate file, it will return a 404 error.
- `location ~ \.php$` — This location block handles the actual PHP processing by pointing Nginx to the `fastcgi-php.conf` configuration file and the `php7.2-fpm.sock` file, which declares what socket is associated with `php-fpm`.
- `location ~ /\.ht` — The last location block deals with `.htaccess` files, which Nginx does not process. By adding the `deny all` directive, if any `.htaccess` files happen to find their way into the document root they will not be served to visitors.

Enable the new server block by creating a symbolic link from the new server block configuration file (in the `/etc/nginx/sites-available/` directory) to the `/etc/nginx/sites-enabled/` directory:

```bash
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```

Then, unlink the default configuration file from the `/sites-enabled/` directory:

```bash
sudo unlink /etc/nginx/sites-enabled/default
```

Test new configuration file for syntax errors:

```bash
sudo nginx -t
```

![enter image description here](https://i.imgur.com/zjMnSEv.png)

use preferred text editor to create a test PHP file called `info.php` in your document root:

```bash
sudo nano /var/www/html/info.php

```

Enter the following lines into the new file. This is valid PHP code that will return information about your server:

```bash
<?php
phpinfo();
```

![enter image description here](https://i.imgur.com/D3i7pot.png)

Now, we can visit this page in your web browser and follow`/info.php` to verify the changes

![enter image description here](https://i.imgur.com/DDQcjIl.png)
