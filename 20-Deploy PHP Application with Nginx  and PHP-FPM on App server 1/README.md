# Task 20 - Deploy PHP Application with Nginx and PHP-FPM on App Server 1

## Objective
Configure a PHP-based application on App Server 1 in Stratos DC using:

- Nginx Web Server
- PHP-FPM 8.3
- Unix Socket Communication
- Custom Port `8092`

---

# Requirements

- Install and configure Nginx
- Configure Nginx to listen on port `8092`
- Set document root to:

```bash
/var/www/html
```

- Install and configure PHP-FPM 8.3
- Configure PHP-FPM socket:

```bash
/var/run/php-fpm/default.sock
```

- Configure Nginx and PHP-FPM integration
- Validate using:

```bash
curl http://stapp01:8092/index.php
```

---

# Step 1: Install Nginx and PHP-FPM

```bash
sudo yum install -y nginx php php-fpm
```

Enable PHP 8.3 module and install packages:

```bash
sudo yum module reset php -y
sudo yum module enable php:8.3 -y
sudo yum install -y php php-fpm php-cli
```

Verify PHP version:

```bash
php -v
```

Expected Output:

```text
PHP 8.3.x
```

---

# Step 2: Configure PHP-FPM

Create socket directory:

```bash
sudo mkdir -p /var/run/php-fpm
```

Edit PHP-FPM pool configuration:

```bash
sudo vi /etc/php-fpm.d/www.conf
```

Configure:

```ini
user = nginx
group = nginx

listen = /var/run/php-fpm/default.sock
listen.owner = nginx
listen.group = nginx
listen.mode = 0660
```

Restart PHP-FPM:

```bash
sudo systemctl restart php-fpm
```

Verify socket:

```bash
sudo ls -l /var/run/php-fpm/default.sock
```

---

# Step 3: Configure Nginx

Edit Nginx configuration:

```bash
sudo vi /etc/nginx/nginx.conf
```

Replace default server block with:

```nginx
server {
    listen       8092;
    listen       [::]:8092;
    server_name  _;

    root /var/www/html;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php-fpm/default.sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}
```

---

# Step 4: Remove Duplicate Configurations

Remove conflicting custom server blocks:

```bash
sudo rm -f /etc/nginx/conf.d/php_app.conf
```

This prevents multiple server blocks from listening on port `8092`.

---

# Step 5: Restart Services

Validate configuration:

```bash
sudo nginx -t
```

Restart services:

```bash
sudo systemctl restart php-fpm
sudo systemctl restart nginx
```

Enable services:

```bash
sudo systemctl enable nginx php-fpm
```

---

# Step 6: Validate Deployment

Verify locally:

```bash
curl http://localhost:8092/index.php
```

Verify from jump host:

```bash
curl http://stapp01:8092/index.php
```

Expected Result:
- PHP application output should display successfully.

---

# Troubleshooting Performed

## Issue 1: PHP 8.0 Installed Instead of 8.3

### Problem
Default PHP version installed was:

```text
PHP 8.0.30
```

### Fix

Enabled PHP 8.3 module and reinstalled packages:

```bash
sudo yum module reset php -y
sudo yum module enable php:8.3 -y
sudo yum install -y php php-fpm php-cli
```

---

## Issue 2: PHP-FPM Socket Ownership Incorrect

### Problem

Socket ownership showed:

```text
root root
```

### Fix

Configured proper socket permissions:

```ini
listen.owner = nginx
listen.group = nginx
listen.mode = 0660
```

---

## Issue 3: Nginx Returning 404

### Root Cause

Multiple server blocks were listening on port `8092`.

Nginx was loading the wrong configuration file.

### Fix

Removed duplicate config file:

```bash
sudo rm -f /etc/nginx/conf.d/php_app.conf
```

---

# Concepts Learned

- Nginx Web Server Configuration
- PHP-FPM Integration
- FastCGI Configuration
- Unix Socket Communication
- Linux Service Management
- Nginx Server Blocks
- PHP Application Deployment
- Troubleshooting 404 Errors
- Socket Permission Management
- Production-style Debugging

---

# Architecture Flow

```text
Browser / curl
        ↓
Nginx :8092
        ↓
Checks /var/www/html/index.php
        ↓
Passes PHP request to:
unix:/var/run/php-fpm/default.sock
        ↓
PHP-FPM executes PHP code
        ↓
Returns generated output
        ↓
Nginx sends response back
```

---

# Final Result

Successfully deployed PHP application on:

```text
http://stapp01:8092/index.php
```

using:

- Nginx
- PHP-FPM 8.3
- Unix Socket Integration
- Custom Port Configuration

---
