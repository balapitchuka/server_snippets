
## Securing Linux Server

- Update your server
  - secure your server by updating the local repositories and upgrade the operating system and installed applications by applying the latest patches
  ```
  On Ubuntu and Debian:
  $ sudo apt update && sudo apt upgrade -y
  
  On Fedora, CentOS, or RHEL:
  $ sudo dnf upgrade
  ```
  
 - Create a new privileged user account
    - create a new user account. You should never log into your server as root. Instead, create your own account ("<user>"), give it sudo rights, and use it to log into your server. 
    ```
     Start out by creating a new user:
     $ adduser <username>

     Give your new user account sudo rights by appending (-a) the sudo group (-G) to the user's group membership:

    $ usermod -a -G sudo <username>
    ```
 - Upload your SSH key
    - You'll want to use an SSH key to log into your new server. You can upload your pre-generated SSH key to your new server using the ssh-copy-id command
  ```
  $ ssh-copy-id <username>@ip_address
  ```
 
- Secure SSH
  - Disable SSH password authentication
  - Restrict root from logging in remotely
  - Restrict access to IPv4 or IPv6
  ```
  Open /etc/ssh/sshd_config using your text editor of choice and ensure these lines:

  PasswordAuthentication yes
  PermitRootLogin yes
  
  look like this:
  PasswordAuthentication no
  PermitRootLogin no
  ```
  - sudo service sshd restart

- Enable a firewall
  - install a firewall, enable it, and configure it only to allow network traffic that you designate. Uncomplicated Firewall (UFW) is an easy-to-use interface to iptables that greatly simplifies the process of configuring a firewall
  ```
  $ sudo apt install ufw
  - By default, UFW denies all incoming connections and allows all outgoing connections. This means any application on your server can reach the internet, but anything trying to reach your server cannot connect.

  First, make sure you can log in by enabling access to SSH, HTTP, and HTTPS:

  $ sudo ufw allow ssh 
  $ sudo ufw allow http
  $ sudo ufw allow https
  
  
   Then enable UFW:

   $ sudo ufw enable
  You can see what services are allowed and denied with:

  $ sudo ufw status
  If you ever want to disable UFW, you can do so by typing:

  $ sudo ufw disable
  ```
