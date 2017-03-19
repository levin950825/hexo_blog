---
title: Setup HTTPS Using Letsencrypt |用Let's Encrypt实现HTTPS
date: 2017-03-17 15:48:02
tags: website dev
categories: Website Dev
comments:
toc: true
thumbnail:
banner:
---

How to use HTTPS for my personal website. Nginx server and Ubuntun 16.04.
<!-- more -->

### My environment
- Web server: Nginx
- Operating System: Ubuntu 16.04 

The steps are mainly following this Digital Ocean Tutorial:
[How To Secure Nginx with Let's Encrypt on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04), but with some modifications for subdomain settings.

### Step 1: Install Let's Encrypt Client
Run the following command on your Ubuntu server:

```bash
sudo apt-get update
sudo apt-get install letsencrypt
```
### Step 2: Obtain an SSL Certificate

Change the Nginx conf files to enable access to `/.well-known` folder:

```bash
sudo nano /etc/nginx/sites-available/peiyingchi.com
```
Add the following line inside the `server` block:

```yml
    location ~ /.well-known {
        allow all;
    }
```

If you want to add the SSL cert to multiple domains or other subdomains, please do the same for all the nginx conf files.

After changing the nginx conf files for the domains you want to setup HTTPS in, run the following:

```bash
sudo letsencrypt certonly -a webroot --webroot-path=/var/www/peiyingchi.com/html -d peiyingchi.com -d www.peiyingchi.com
```

To setup for subdomains together:

```bash
sudo letsencrypt certonly --webroot -w /var/www/peiyingchi.com/html/ -d www.peiyingchi.com -d peiyingchi.com -w /var/www/blog.peiyingchi.com/html -d blog.peiyingchi.com
```
You will be prompted to enter your contact email and agree some terms and conditions.

My cert files are at :

```bash
$ yingchi@yingchi-site:~$ sudo ls -l /etc/letsencrypt/live/peiyingchi.com

total 0
lrwxrwxrwx 1 root root 38 Mar 17 01:48 cert.pem -> ../../archive/peiyingchi.com/cert1.pem
lrwxrwxrwx 1 root root 39 Mar 17 01:48 chain.pem -> ../../archive/peiyingchi.com/chain1.pem
lrwxrwxrwx 1 root root 43 Mar 17 01:48 fullchain.pem -> ../../archive/peiyingchi.com/fullchain1.pem
lrwxrwxrwx 1 root root 41 Mar 17 01:48 privkey.pem -> ../../archive/peiyingchi.com/privkey1.pem
```

After obtaining the cert, you will have the following PEM-encoded files:

- `cert.pem`: Your domain's certificate
- `chain.pem`: The Let's Encrypt chain certificate
- `fullchain.pem`: `cert.pem` and `chain.pem` combined
- `privkey.pem`: Your certificate's private key

To further increase security, you should also generate a strong **Diffie-Hellman group**. To generate a 2048-bit group, use this command:

```bash
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```

### Step 3: Configure TLS/SSL on Web Server (Nginx)
Now that you have an SSL certificate, you need to configure your Nginx web server to use it.

We will make a few adjustments to our configuration:

1. We will create a configuration snippet containing our SSL key and certificate file locations.
2. We will create a configuration snippet containing strong SSL settings that can be used with any certificates in the future.
3. We will adjust the Nginx server blocks to handle SSL requests and use the two snippets above.

First, let's create a new Nginx configuration snippet in the `/etc/nginx/snippets` directory.

To properly distinguish the purpose of this file, we will name it `ssl-` followed by our domain name, followed by `.conf` on the end:

```bash
sudo nano /etc/nginx/snippets/ssl-peiyingchi.com.conf
```

```conf ssl-peiyingchi.com.conf
ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
```

Next, we will create another snippet that will define some SSL settings. This will set Nginx up with a strong SSL cipher suite and enable some advanced features that will help keep our server secure.

The parameters we will set can be reused in future Nginx configurations, so we will give the file a generic name:

```bash
sudo nano /etc/nginx/snippets/ssl-params.conf
```

```conf ssl-params.conf
# from https://cipherli.st/
# and https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html

ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";
ssl_ecdh_curve secp384r1;
ssl_session_cache shared:SSL:10m;
ssl_session_tickets off;
ssl_stapling on;
ssl_stapling_verify on;
resolver 8.8.8.8 8.8.4.4 valid=300s;
resolver_timeout 5s;
# Disable preloading HSTS for now.  You can use the commented out header line that includes
# the "preload" directive if you understand the implications.
#add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
add_header Strict-Transport-Security "max-age=63072000; includeSubdomains";
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;

ssl_dhparam /etc/ssl/certs/dhparam.pem;
```

> If you are interested, you can take take a moment to read up on [HTTP Strict Transport Security, or HSTS](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security), and specifically about the ["preload"](https://hstspreload.appspot.com/) functionality. Preloading HSTS provides increased security, but can have far reaching consequences if accidentally enabled or enabled incorrectly. In this guide, we will not preload the settings, but you can modify that if you are sure you understand the implications

Now that we have our snippets, we can adjust our Nginx configuration to enable SSL.

Before we go any further, let's back up our current server block file:

```bash
sudo cp /etc/nginx/sites-available/peiyingchi.com /etc/nginx/sites-available/peiyingchi.com.bak
```
Now, open the server block file to make adjustments.
Blow are the 2 conf files for my top-level domain and my subdomain:

```bash
sudo nano /etc/nginx/sites-available/peiyingchi.com
```

```js peiyingchi.com
server {
	listen 80;
	listen [::]:80;

	server_name peiyingchi.com www.peiyingchi.com;
	return 301 https://$server_name$request_uri;
}

server {

	# SSL configuration
	listen 443 ssl http2 default_server;
	listen [::]:443 ssl http2 default_server;
	include snippets/ssl-peiyingchi.com.conf;
	include snippets/ssl-params.conf;
	
	root /var/www/peiyingchi.com/html;

    # Add index.php to the list if you are using PHP
	index index.html index.php;
	
	server_name peiyingchi.com www.peiyingchi.com;
	
        location / {
            try_files $uri $uri/ =404;
        }

        location ~ /.well-known {
            allow all;
        }

}
```

```js blog.peiyingchi.com
server {
	listen 80;
	listen [::]:80;

	server_name blog.peiyingchi.com www.blog.peiyingchi.com;
	return 301 https://$server_name$request_uri;
}

server {
	# SSL configuration

	listen 443 ssl http2;
	listen [::]:443 ssl http2;
	include snippets/ssl-peiyingchi.com.conf; # NOTE: the same file as the top level domain
	include snippets/ssl-params.conf;	
	
	root /var/www/blog.peiyingchi.com/html;
	index index.html;
	server_name blog.peiyingchi.com www.blog.peiyingchi.com;
	
	location / {
		try_files $uri $uri/ =404;
	}
	location ~ /.well-known {
		allow all;
	}
}
```

### Step 4: Adjust the Firewall
If you have the `ufw` firewall enabled, as recommended by the prerequisite guides, you'll need to adjust the settings to allow for SSL traffic.

You can see the current setting by typing:

```bash
sudo ufw status
```

To additionally let in HTTPS traffic, we can allow the "Nginx Full" profile and then delete the redundant "Nginx HTTP" profile allowance:

```bash
sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
```

### Step 5: Enabling the Changes in Nginx
First, we should check to make sure that there are no syntax errors in our files. We can do this by typing:

```bash
sudo nginx -t
```

If your output matches the above, your configuration file has no syntax errors. We can safely restart Nginx to implement our changes:

```bash
sudo systemctl restart nginx
```
You can use the Qualys SSL Labs Report to see how your server configuration scores:
Enter the following to your web browser:

https://www.ssllabs.com/ssltest/analyze.html?d=<span style="color:red">example.com</span>

### Step 6: Set Up Auto Renewal
From Let's Encrypt's [official documentation](https://certbot.eff.org/#ubuntuxenial-nginx), you can run the following to test the renew function:

```bash
sudo letsencrypt renew --dry-run --agree-tos
```
> Do not worry for the 'Registering without email!' warning. It does not hurt anything.

If that appears to be working correctly, you can arrange for automatic renewal by adding a cron or systemd job which runs `letsencrypt renew`

Since the renewal first checks for the expiration date and only executes the renewal if the certificate is less than 30 days away from expiration, it is safe to create a cron job that runs every week or even every day, for instance.

Let's edit the crontab to create a new job that will run the renewal command every week. To edit the crontab for the root user, run:

```bash
sudo crontab -e
```

Add the following lines:

```sh crontab entry
30 2 * * 1 /usr/bin/letsencrypt renew >> /var/log/le-renew.log
35 2 * * 1 /bin/systemctl reload nginx
```
This will create a new cron job that will execute the letsencrypt-auto renew command every Monday at 2:30 am, and reload Nginx at 2:35am (so the renewed certificate will be used). The output produced by the command will be piped to a log file located at `/var/log/le-renewal.log`.


