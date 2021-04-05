# Nginx Basics

- Recommended OS - Linux


### nginx configuration files in os

#### Ubuntu
/etc/nginx/

### Install Nginx
- apt-get update
- sudo apt-get install nginx
- Check whether nginx is running
    - aux | grex nginx
    - ifconfig
       - navigate to ip address
    - sudo systemctl status nginx
    - service nginx [start/stop/status] 

### Installation Troubleshoot
- [Apache Default Page Showing](https://askubuntu.com/questions/642238/why-do-i-still-see-an-apache-site-on-nginx/642288#642288)

### Configuration Terms
    - Directive
        - Example : server_name hello.com;
            
    - Context or Scope
        - Sections with in configuration where directives are set
        - http context 
        - server context
        - events context

### Reload Nginx(no downtime)
sudo systemctl reload nginx

### Restart Nginx
sudo systemctl restart

### Check for configuration page errors
nginx -t

### Create a virtual host
```
events {
 
}

http {
   
  include mime.types;

   server {
     listen 80;
     server_name  192.168.1.105;
     
     root /sites/demo;
    }
}


```

### Location Blocks (order in which we mention location blocks here matters)

- Exact Match
   -  = uri
- Preferential Prefix Match
   - ^~ uri
- REGEX Match
    - ~* uri
   ```
   # REGEX match - case sensitive
    location ~ /greet[0-9] {
       return 200 'Hello from NGINX "/greet" location - REGEX MATCH.';
     }

     # REGEX match - case insensitive
    location ~* /greet[0-9] {
      return 200 'Hello from NGINX "/greet" location - REGEX MATCH INSENSITIVE.';
    }
    ```
- Prefix Match
    - uri

# Variables
-  Configuration Variables
    - set $var 'something'
- Nginx Module Variables
    - $http, $uri, $args

- Query Parameters will be made available as individual arguments by nginx
    - ?name=ironman&&age=30
        - $arg_name
        - $arg_age

### return and rewrite in nginx
- rewrite
    - **rewrite pattern URI**
- return 
    - ***return statuscode URI**
