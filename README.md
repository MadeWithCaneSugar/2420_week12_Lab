# 2420 Week 12 Lab Readme
Justin Tan
Mitchel Webb
## Usage
To set up a droplet serving a static html file with a firewall

### Part 1 - Installing NGINX
Start by running the apt install script for NGINX:
```
sudo apt install nginx
```

respond `y` to the prompt following that command

### Part 2 - Creating an HTML file

#### Creating the file in your droplet
To create the `index.html` on your droplet you will need to first be in the html directory. You can do this by running the command: `cd /var/www/html`

From here you can create the html file with the command: `sudo vim index.html`. You will need to use sudo or else vim won't let you write to the file.

#### Creating your file on your host machine
For the purposes of uploading to Github more easily, I've created this on my host machine and will use `sftp` to copy the file over.

Your html file should look similar to this:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>This is the 'index.html' file</h1>
    <h2>Welcome</h2>
</body>
</html>
```

#### If Using Host Machine
You'll need to run `sftp` to transfer the html file over to your droplet:
```
sftp -i [path to ssh key] [user name]@[IP address]
```

for me this command looks like this(run the command when already in the directory you have your index.html in):
![Script](images/sftp_connect.PNG)

Once inside this `sftp`, type
```
put index.html
```
This will copy your index.html file from the directory you made it in to the home directory of your droplet. We need this file to be in `/var/www/html/` for the next step though, so move it using
```
mv index.html /var/www/html/
```

### Part 3 - NGINX
For this step, you're going to create an NGINX server file. Like in the previous step, you can create this in your droplet by typing
```
cd /etc/nginx/sites-avalable
```
followed by
```
vim [server block name]
```

Again for purposes of the Github repo, I'll be making this on my host machine and moving it over in the same way I moved the `index.html`

You can call your file whatever you like; I'm going to call mine `week_12_server_block`. The content of your file should look like this:
```
server {
        listen 80;
        listen [::]:80;

        root /var/www/137.184.173.46/html;
        index index.html;

        server_name 137.184.173.46;

        location / {
                try_files $uri $uri/ =404;
        }
}
```
