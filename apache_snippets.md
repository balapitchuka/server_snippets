# Apache Snippets

+ In Red-Hat based distros such as CentOS and Fedora, virtual host files are stored in the `/etc/httpd/conf.d`. 
+ While on Debian and its derivatives like Ubuntu the files are stored in the `/etc/apache2/sites-available directory`.

## Installation

### Windows
[Install Apache For Windows](https://www.apachelounge.com/download/)

After making configuration changes
+ go to bin directory and run `httpd -t`, checks for configuration syntax correctness

##### To Redirect A Website To HTTPS
```
<VirtualHost *:80> 
  ServerName example.com
  ServerAlias www.example.com

  Redirect permanent / https://example.com/
</VirtualHost>

<VirtualHost *:443>
  ServerName example.com
  ServerAlias www.example.com

  Protocols h2 http/1.1

  # SSL Configuration

  # Other Apache Configuration

</VirtualHost>

```

##### Reload Apache Server
###### Whenever you make changes to the configuration files you need to restart or reload the Apache service for changes to take effect:
```
Debian and Ubuntu:

sudo systemctl reload apache2

CentOS and Fedora:

sudo systemctl reload httpd
```
