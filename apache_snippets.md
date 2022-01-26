# Apache Server

+ In Red-Hat based distros such as CentOS and Fedora, virtual host files are stored in the `/etc/httpd/conf.d`. 
+ While on Debian and its derivatives like Ubuntu the files are stored in the `/etc/apache2/sites-available directory`.




## Installation

### Windows
[Install Apache For Windows](https://www.apachelounge.com/download/)

After making configuration changes
+ go to bin directory and run `httpd -t`, checks for configuration syntax correctness

### Linux
- Ubuntu
  - `sudo apt install apache2` 
  - By default, Ubuntu will immediately start and enable the apache2 daemon as soon as its package is installed

## Apache Server Terminology
- **Directive**
  - A configuration command that controls one or more aspects of Apache's behavior. 
  - Directives are placed in the Configuration File
- **Configuration File**
  - A text file containing Directives that control the configuration of Apache.



## Apache Server Directives
- ServerSignature
  - Permits the adding of a footer line showing server name and version number under server-generated documents    such as error messages, mod_proxy ftp directory listings, mod_info output plus many more.
  - It has three possible values:
    - On – which allows the adding of a trailing footer line in server-generated documents,
    - Off – disables the footer line and
    - EMail – creates a “mailto:” reference; which sends a mail to the ServerAdmin of the referenced document.

- ServerTokens
  - It determines if the server response header field that is sent back to clients contains a description of the server OS-type and info concerning enabled Apache modules.
  - Possible values
  ```
  ServerTokens   Full (or not specified) 
  Info sent to clients: Server: Apache/2.4.2 (Unix) PHP/4.2.2 MyMod/1.2 

  ServerTokens   Prod[uctOnly] 
  Info sent to clients: Server: Apache 

  ServerTokens   Major 
  Info sent to clients: Server: Apache/2 

  ServerTokens   Minor 
  Info sent to clients: Server: Apache/2.4 

  ServerTokens   Min[imal] 
  Info sent to clients: Server: Apache/2.4.2 

  ServerTokens   OS 
  Info sent to clients: Server: Apache/2.4.2 (Unix)
  ```
  
## Apache configuration files
- Debian/Ubuntu systems
  - sudo vi /etc/apache2/apache2.conf
- RHEL/CentOS systems 
  - sudo vi /etc/httpd/conf/httpd.conf       
  
## Snippets

- Apache config test
  ```
  apachectl -t
  ```
- Restart apache server
  ```
  > sudo systemctl restart apache2  #SystemD
  > sudo service apache2 restart     #SysVInit
  ```

- Check which port apache is running
  ```
  In Debian
  > netstat -tlpn| grep apache
  > ss -tlpn| grep apache
  
  In RHEL
  > netstat -tlpn| grep httpd
  > ss -tlpn| grep httpd
  ```
  
- Change Apache server HTTP port
  ```
  Debian/Ubuntu based system  `/etc/apache2/ports.conf`
  RHEL/CentOS based distributions  `/etc/httpd/conf/httpd.conf file`
  ```

- Enable site `a2ensite` command
  ```
  The a2ensite command creates a symbolic link to the particular configuration file from sites-enabled to 
  sites-available. 
  
  If you have already enabled a site's config and make modifications to it you do not need to enable it again and can simply 
  use graceful or reload

  1.The apache2.conf file has an include for sites-enabled/*
  2.You make a site specific config file in sites-available/
  3.When you run a2ensite it creates a symbolic link to the file from sites-enabled to sites-available thus making Apache 
  pick it up from the sites-enabled/* include. All a2dissite does is delete the symlink.

  So a2ensite is effectively just 
  > ln -s /etc/apache2/sites-available/sitename.conf /etc/apache2/sites-enabled/sitename.conf. 
  
  So, once it has been done changes to the already linked file do not affect the link you just need Apache to reload config.
  ```
  - Enabling site with a2ensite
    - a2ensite /etc/apache2/sites-available/new-domain.conf
  - Disabling site with a2dissite
    - a2dissite /etc/apache2/sites-available/new-domain.conf
 
- Setting up Apache virtual hosts
  - OnceApache is installed and running, you can configure it to serve multiple domains by using virtual hosts.




- To Redirect A Website To HTTPS
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

- Reload Apache Server
  - Whenever you make changes to the configuration files you need to restart or reload the Apache service for changes to take effect:
  ```
  Debian and Ubuntu:

  sudo systemctl reload apache2

  CentOS and Fedora:

  sudo systemctl reload httpd
  ```

- Avoid sending os and apache version for not found routes
  ```
  nano /etc/apache2/conf-available/security.conf

  change ServerTokens OS to ServerTokens Prod
  change ServerSignature On to ServerSignature Off

  sudo systemctl restart apache2
  ```

- View Loaded Apache Modules
  ```
  apache2ctl -M

  apache2ctl -M | sort
  ```
