First lets install nginx with the following command

```bash
sudo apt install nginx
```

After installing, we can check the status nginx service with following command

```bash
sudo systemctl status nginx
```

![enter image description here](https://i.imgur.com/Tp9jkTq.png)

Now lets create a simple html file and keep this file on the following directory

```
/var/www/localhost
```

![enter image description here](https://i.imgur.com/rTEyXIs.png)

Now create localhost file inside `/etc/nginx/sites-available` directory and keept the following nginx configuration

```
server {
    listen      80;
    listen      [::]:80;
    server_name localhost;
    root        /var/www/localhost;


    # index.html fallback
    location / {
        try_files $uri $uri/ /index.html;
    }

}

```

![enter image description here](https://i.imgur.com/SMOO3nn.png)

Now lets create symlink of this configuration file to the sites-enabled folder

```bash
sudo ln -s /etc/nginx/sites-available/localhost /etc/nginx/sites-enabled/
```

We can check if the configuration file is valid by using the following command

```bash
sudo nginx -t
```

To see our changes we have to restart our nginx service, which can be done by using the following command

```
sudo systemctl restart nginx
```

To see our changes lets visit `localhost` on our browser

![enter image description here](https://i.imgur.com/UG2KeUB.png)
