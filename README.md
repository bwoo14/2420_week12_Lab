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
![](images/sudo-apt-update.png)
- Then upgrade with:
```
sudo apt upgrade
```
![](images/sudo-apt-upgrade.png)
- You might have to restart some libraries. In that case just the default (press TAB and hit ENTER)
![](images/updateconfiguration2.png)

- Run the command:
```
sudo apt install nginx
```
![](images/sudo-apt-install-nginx.png)

- Again, Ubuntu might ask you to restart libraries. Hit TAB and press ENTER
![](images/updateconfiguration3.png)

### Step 2: Create an HTML Document
- On your host machine, create an HTML document with the following code named `index.html`:
```
<!DOCTYPE html>
<html lang="en">
<html>
    <head>
        <meta charset="UTF-8" />
        <title>My Landing Page</title>
    </head>
    <body>
        <h1>Welcome to Brandon, Doris, and Parnian's webpage </h1>
        <b>Your cookies are ours >:)</b>
    </body>
</html>
```
![](images/index-content.png)

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

![](images/server-block.png)

### Step 4: Upload files to server using SFTP and Moving Files
- Connect to your server using sftp on your local host with:
```
sftp -i "~/.ssh/DO_key" puddle@165.232.155.129
```
![](images/connect-stfp.png)

- Use the `put` command to move the index.html file and the server block file to the server
```
put index.html
put 165.232.155.129
```
![](images/put-commands.png)
- The files should be in your server's home directory
![](images/sftp-success.png)

- ssh into your server. Then, move the server block file to `/etc/nginx/sites-available`
```
sudo mv 165.232.155.129 /etc/nginx/sites-available
```
![](images/mv-server.png)

- run the command:

```
sudo mkdir /var/www/165.232.155.129 && sudo mkdir /var/www/165.232.155.129/html
```
![](images/mkdir.png)

- move the index.html file to the newly created directory

```
sudo mv index.html /var/www/165.232.155.129/html
```
![](images/mv-index.png)

- Create a symbolic link to the new server block
```
sudo ln -s /etc/nginx/sites-available/165.232.155.129 /etc/nginx/sites-enabled/
```
![](images/soft-link.png)

- Make sure that your nginx syntax is correct with:
```
sudo nginx -t
```
![](images/successful-nginx.png)

### Step 5: Starging the Service
- Reload your daemon:
```
sudo systemctl daemon-reload
```
![](images/reload.png)

- Start the nginx service
```
sudo systemctl start nginx
```
![](images/start-service.png)

- Reload the nginx service
```
sudo sysctemctl reload nginx
```
![](images/reload.png)

### Step 6: Check to see if you can connect to the server via HTTP
- Visit the IP in your browser: It should look like this
![](images/first-success.png)

### Step 7: Create the Firewall
- Go to the DigitalOcean website and select your project
- Click the create button in the top right
![](images/create.png)
- Scroll down and select cloud firewall
![](images/cloud-firewall.png)
- Name the firewall 'web-one-firewall'
- In the "Inbound rules" select new rule and choose the HTTP option
![](images/http.png)
> This will allow all HTTP requests to go through to your server. SSH will already be there by default. Everything else will be dropped
![](images/httpdone.png)
- Scroll down and apply the firewall to your droplet
![](images/apply.png)

## Step 8: Checking if everything works
- Check you can still connect to the server on your browser
![](images/first-success.png)

- Ensure you can still ssh into the server
![](images/ssh.png)


