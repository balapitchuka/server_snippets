# Nginx Basics


## References
- [Using Free Let’s Encrypt SSL/TLS Certificates with NGINX](https://www.nginx.com/blog/using-free-ssltls-certificates-from-lets-encrypt-with-nginx/)


Recommended OS - Linux


## Nginx configuration files in os

### Ubuntu
/etc/nginx/

NginxDirectoryStructure

|  path | purpose  |
|---|---|
| ./conf.d/*.conf  | Extra configuration files.|
| ./nginx.conf  |  The primary configuration file. |
| ./sites-available/*  |  Extra virtual host configuration files |
| ./sites-enabled/* | Symlink to sites-available/<file> to enable vhost |
| ./snippets/*.conf | Configuration snippets that can be included in configs |
| ./mime.types | Maps file name extensions to MIME types of responses |
| ./proxy_params | Commonly configured directives | 
| ./scgi_params | Commonly configured directives |
| ./apps.d/*.conf | Files included by /etc/nginx/sites-available/default |

    

**/etc/nginx/nginx.conf**
+ The first file that nginx reads when it starts is /etc/nginx/nginx.conf. This file is maintained by Nginx package maintainers and it is recommended that administrators avoid editing this file unless they also follow changes made by upstream.
+ It's advised to instead add customizations underneath of the conf.d/

**/etc/nginx/conf.d/*.conf**
+ The default nginx.conf file includes a line that will load additional configurations files into the http { } context. In most cases, options previously specified in the primary nginx.conf file can be overridden by creating a file at this location.
```
Example: /etc/nginx/conf.d/ssl-tweaks.conf

add_header Strict-Transport-Security "max-age=31536000; includeSubdomains";
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;
ssl_ciphers 'kEECDH+CHACHA kEECDH+AESGCM HIGH+kEECDH AESGCM 3DES !SRP !PSK !DSS !MD5 !LOW !MEDIUM !aNULL !eNULL !DH !kECDH';   
```




## Install Nginx
- apt-get update
- sudo apt-get install nginx
- Check whether nginx is running
    - aux | grex nginx
    - ifconfig
       - navigate to ip address
    - sudo systemctl status nginx
    - service nginx [start/stop/status] 

## Installation Troubleshoot
- [Apache Default Page Showing](https://askubuntu.com/questions/642238/why-do-i-still-see-an-apache-site-on-nginx/642288#642288)

## Configuration Terms
- Directive
    - Example : server_name hello.com;

- Context or Scope
    - Sections with in configuration where directives are set
    - http context 
    - server context
    - events context



##### Reload Nginx(no downtime)
```
sudo systemctl reload nginx
```

##### Restart Nginx
```
sudo systemctl restart nginx
```

##### Check for configuration page errors
```
nginx -t
```

##### Hide nginx server version on Linux
```
1. sudo nano /etc/nginx/nginx.conf

2. Add the following line to http context
server_tokens off;

3. Restart the nginx server
sudo systemctl restart nginx
```

##### Hide nginx server name on Linux
- compile Nginx from sources and include the --build=name option to set a nginx build name.

##### Create a virtual host
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


##### Redirect non-SSL to SSL
```
server {
    listen   80;
    listen   [::]:80;
    listen   443 default_server ssl;

    server_name www.example.com;

    ssl_certificate        /path/to/my/cert;
    ssl_certificate_key  /path/to/my/key;

    if ($scheme = http) {
        return 301 https://$server_name$request_uri;
    }
}
```


## Nginx Reverse Proxy
+ The reverse proxy setup of Nginx can be really useful in load balancing external requests to microservices
+ Nginx is frequently used for setting up as a reverse proxy server. 
+ When set as a reverse proxy server, Nginx receives requests, passes them to the proxied servers, 
  retrieves responses from the proxied servers, and sends them back to the clients.

##### Configure HTTP Reverse Proxy
An HTTP reverse proxy can be set up in Nginx to proxy requests with the HTTP protocol. Relevant sections in nginx.conf for setting up an HTTP proxy are 
```
http {
    upstream myapp1 {
        server localhost:8081;
        server localhost:8082;
    }
    server {
        listen       8080;
        server_name  localhost;
        location / {
            proxy_pass http://myapp1;
        }
    }

Nginx HTTP Reverse Proxy Configuration
```
Now, an HTTP request similar to URL pattern http://localhost:8080 will be proxied to both of the following URLs in a load-balanced fashion:
+ http://localhost:8081
+ http://localhost:8082

##### Configure TCP Reverse Proxy
```
stream {
    upstream myapp1 {
        server localhost:5672;
        server localhost:5673;
    }
    server {
        listen     5671;
        proxy_pass myapp1;
    }
}
```
