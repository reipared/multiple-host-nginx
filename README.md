# Host Multiple Websites On One Server with a Single IP Address

## Setup

### 1. Install NGINX

`sudo apt install nginx`

### 2. Create Directories

- Create directories for your websites:

  ```shell
  sudo mkdir -p /var/www/example.com/public_html/
  sudo mkdir -p /var/www/test.com/public_html
  ```

### 3. Set Ownership and Permissions

- Set ownership and permissions for the directories:

  ```shell
  sudo chwon -R user:user /var/www/example.com/public_html/
  sudo chwon -R user:user /var/www/test.com/public_html/
  ```

  ```shell
  sudo chmod 775 /var/www/example.com/public_html/
  sudo chmod 775 /var/www/test.com/public_html/
  ```

### 4. Copy HTML Files

- Copy your HTML files and name them index.html

  ```shell
  sudo cp example.html /var/www/example.com/public_health/index.html
  sudo cp test.html /var/www/test.com/public_health/index.html
  ```

## Configuring NGINX

### 1. Navigate to Sites Configuration

- Navigate to the directory `/etc/nginx/sites-available`.

### 2. Create Site Configurations

- Create configuration files for example.com and test.com:

  ```shell
  sudo nano example.com
  sudo nano test.com
  ```

### 3. Configure example.com

- Edit `example.com` configuration:

  ```nginx
  server {
    listen *:80 default_server;
    listen [::]:80 default_server;
    server_name example.com;
    location / {
      root /var/www/example.com/public_html;
      index index.html;
    }
  }
  ```

### 4. Configure test.com

- Edit `test.com` configuration:

  ```nginx
  server {
    listen *:80;
    listen [::]:80;
    server_name test.com;
    location / {
      root /var/www/test.com/public_health;
      index index.html;
    }
  }
  ```

- `test.com` configuration file doesn't have `default_server` because there can only be one default server.

### 5. Enable the Sites

- Navigate to the sites-enabled directory:

  `cd /etc/nginx/sites-enabled/`

- Check if there are any files in the directory

  `ls`

- If its not empty, delete any configuration file there, usually the `default` file is there.

- If it's empty, go back to sites-available:

  `cd /etc/nginx/sites-available`

- Create symbolic links for the configurations:

  ```shell
  sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/example.com
  sudo ln -s /etc/nginx/sites-available/test.com /etc/nginx/sites-enabled/test.com
  ```

## Restart NGINX

`sudo systemctl restart nginx`

## Emulate a Small DNS

- Edit your hosts file:

  `sudo nano /etc/hosts`

- Add the following lines:

  ```shell
  127.0.0.1 example.com
  127.0.0.1 test.com
  ```

## Verify DNS

- Use `nslookup` to verify that both domains points to the IP 127.0.0.1:

  ```shell
  nslookup example.com
  nslookup test.com
  ```

- Open a web browser and type `example.com` and `test.com` to verify that they are working.

- If the website opens to somewhere else, clear the cache and try again.

This structured README should help users follow the steps to host multiple websites on a single server with a single IP address using NGINX. You can further enhance it by adding explanations and troubleshooting tips as needed.
