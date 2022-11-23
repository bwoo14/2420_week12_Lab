# 2420_week12_Lab
By Brandon Woo, Yingyi (Doris) He, Parnian Azarm

## Prerequisites
- A successful DO Server with ssh
- Sudo access on the server


## 1. Tutorial

### Step 1: Install Nginx
- Before installing Nginx, you should update your server. Use this command on your Digital Ocean Server:
```
sudo apt update
```
![](sudo-apt-update.png)
- Then upgrade with:
```
sudo apt upgrade
```
![](sudo-apt-upgrade.png)
- You might have to restart some libraries. In that case just the default (press TAB and hit ENTER)
![](updateconfiguration2.png)

- Run the command:
```
sudo apt install nginx
```
![](sudo-apt-install-nginx.png)

- Again, Ubuntu might ask you to restart libraries. Hit TAB and press ENTER
![](updateconfiguration3.png)

### Step 2: Create an HTML Document
- On your host machine, create an HTML document with the following code named `index.html`:
```
<!DOCTYPE html>
<html lang="en">
<html>
    <head>
		<meta charset="UTF-8" />
        <title>Example Site for 2420</title>
    </head>
    <body>
        <h1>Success!</h1>
        <h2 style="color: red;">All your internets are belong to us!</h2>
    </body>
</html>
```
![](index-content.png)

### Step 3: Write an nginx server block
- Create an nginx server block file (named with your server's ip address e.g., 165.232.155.129) with the following code:
```
server {
        listen 80;
        listen [::]:80;

        root /var/www/165.232.155.129/html;
        index index.html;

        server_name 165.232.155.129;

        location / {
                try_files $uri $uri/ =404;
        }
}
```
>Make sure to change the `server_name` value to the IP address of your digital ocean server. Change the `root` value as well.

![](server-block.png)

