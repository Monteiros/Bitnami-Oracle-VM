# Bitnami-Wordpress-stack-installation-in-Oracle-VM---Cheat-sheet

A step by step guide to installation of Bitnami stack on Oracle VM instance 

Valid under Ubunto, CentOS, Fedora or Red Hat Enterprise Linux
-------------------------------------------------------------

# STEP 1: 
First, get root access using this command:

    sudo -i

# STEP 2: 
We have to allow added ports in the system firewall. Use this to allow an HTTP & HTTPS ports:

    sudo firewall-cmd --permanent --zone=public --add-port=80/tcp

    sudo firewall-cmd --permanent --zone=public --add-port=443/tcp


# STEP 3: 
reload the firewall to make changes active:

    sudo firewall-cmd --reload


# STEP 4: 
update the list of packages of CentOS 7 or whatever Linux distro using this command:

    sudo yum update


# STEP 5: 
check the memory using this command to make sure, it has a swap file:

    free -m

# STEP 6: 
Install Bitnami dependencies

/* if  Ubunto*/

    sudo apt-get update
    sudo apt-get install libncurses5

/* if CentOS, Fedora or RHEL

    sudo yum install ncurses-compat-libs

# STEP 7: 
install Perl. So the server got prepared to run the WordPress setup.

/* if Ubunto*/
    
    sudo apt-get install perl
    
/* if CentOS/Fedora/RHEL*/

    sudo yum install perl perl-Data-Dumper

# STEP 8: 
get the latest version of Bitnami at the official site -just copy the download link

    wget https://bitnami.com/redirect/to/1448613/bitnami-wordpress-5.7.1-0-linux-x64-installer.run

# STEP 9: 
Provide execute permission on the installer package using this command.

    sudo chmod 755 bitnami-wordpress-5.7.1-0-linux-x64-installer.run

# STEP 10: 
execute the installer.*/

    ./bitnami-wordpress-5.7.1-0-linux-x64-installer.run



# Bonus----------

# 1 Remove Bitnami Welcome page

    /opt/wordpress-5.9-0/apps/wordpress/bnconfig --appurl /

# 2 Remove Bitnami banner at the bottom right corner

    sudo /opt/wordpress-5.9-0/apps/wordpress/bnconfig --disable_banner 1

# 3 restart the apache server for changes to take effect

    sudo /opt/wordpress-5.7.1-0/ctlscript.sh restart apache


Now all you need is to point your domain to your machine public ip by adding two DNS records:

# “A” record
@ public IP

# CNAME record
www @

# Generate a Free SSL certificate and force redirection from HTTP to HTTPS, www to non-www
sudo /opt/wordpress-5.7.1-0/bncert-tool

# ENJOY YOUR FREE WORDPRESS HOSTING ! 🎉

