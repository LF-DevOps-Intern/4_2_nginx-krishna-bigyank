Spwan 4 node server with the following command

```
pm2 start server1.js -i 4
```

![enter image description here](https://i.imgur.com/kITjOxf.png)

These servers are running on the port 6080. We can verify this by visitng `localhost:6090` on our browser

![enter image description here](https://i.imgur.com/Xm2lOab.png)

Now lets proxy pass to this port from the **api.localhost** to port 6080. To do this add the following configuration on sites-available directory.

![enter image description here](https://i.imgur.com/jbqlkkz.png)

In addition to this, lets create another config file inside proxy directory which will contain necessary proxy headers.

Enable the new server block by creating a symbolic link from the new server block configuration file (in the `/etc/nginx/sites-available/` directory) to the `/etc/nginx/sites-enabled/` directory:

```bash
sudo ln -s /etc/nginx/sites-available/node /etc/nginx/sites-enabled/
```

Restart the nginx service

```
sudo systemctl restart nginx
```

We can verify that the reverse proxy is working by visiting the domain **api.localhost** on our local browser

![enter image description here](https://i.imgur.com/x4pS1Eg.png)

Further we can dissallow the port 6080 on our firewall so that only connction through nginx is allowed.
