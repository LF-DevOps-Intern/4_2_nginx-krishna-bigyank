lets create reverse_proxy.conf file in `/etc/nginx/sites-available` directory and put the following configuration inside

```
server {
    listen 85;
    server_name localhost;
    location / {
        proxy_pass http://localhost:82/;
    }
}
```

![enter image description here](https://i.imgur.com/Bedn29g.png)

create a link so that nginx can find the new configuration

```bash
sudo ln -s /etc/nginx/sites-available/reverse_proxy /etc/nginx/sites-enabled/
```

Lets restart nginx and verify it in our browser

![enter image description here](https://i.imgur.com/BLD66BA.png)

![enter image description here](https://i.imgur.com/7tSCRQN.png)
