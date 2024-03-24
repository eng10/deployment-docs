# deployment-docs


## Update the server
``` 
sudo apt-get update $$ sudo apt-get upgrade
```


## Install Nginx

``` 
apt install nginx
```
### Test Nginx
```
sudo nginx -t
```

## Create main project directory
``` 
cd /var/www/ && sudo mkdir <your-website-name>
```

## Clone your project
``` 
git clone <project-url>
```
## Install Node Js
```
1. Install Curl
sudo apt install -y curl

2. Install NodeJs repository and dependencies
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

3. Install NodeJs
sudo apt install -y nodejs

```

## Build frontend
``` 
cd frontend
```
``` 
sudo npm run build
```
### Enable firewall with ufw
```
sudo apt install ufw
```

```
ufw enable
```

### Allow Nginx
```
ufw allow "Nginx Full"
```

### Remove default files
```
 rm /etc/nginx/sites-available/default
```
```
 rm /etc/nginx/sites-enabled/default
```

### Create default configuration files
```
sudo nano /etc/nginx/sites-available/<your-project-name>
```

### Sycnc default files sites-enabled and sites-available
``` 
ln -s /etc/nginx/sites-available/<your-project-name> /etc/nginx/sites-enabled/<your-project-name>
```
### Deploy frontend build files

```
server {
 listen 80;
 server_name your_domain.com www.your_domain.com;

location / {
 root /var/www/<project-name>/frontend/dist;
 index  index.html index.htm;
 proxy_http_version 1.1;
 proxy_set_header Upgrade $http_upgrade;
 proxy_set_header Connection 'upgrade';
 proxy_set_header Host $host;
 proxy_cache_bypass $http_upgrade;
 try_files $uri $uri/ /index.html;
}
```

### posrgres setup

```

CREATE ROLE userName;
ALTER ROLE userName WITH ENCRYPTED PASSWORD 'userPassword';
GRANT ALL PRIVILEGES ON DATABASE databaseName TO userName;

```

### Deploy backend
```
server {
  listen 80;
  server_name api.your_domain.com;
  location / {
    proxy_pass http://localhost:5000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
    }
}
```


