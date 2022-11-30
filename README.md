
# Bitnami-Wordpress-stack-installation-in-Oracle-VM---Cheat-sheet

This step-by-step procedure is for the WordPress packaged by Bitnami - a one-click install solution,but you can go ahead and download whatever tool you want.

Of course you'll first need an account at [Oracle cloud](https://signup.cloud.oracle.com/), and perhaps you'd like to read this [guide](https://blogs.oracle.com/lad-cloud-experts-pt/post/o-que-e-oracle-cloud-always-free-services).
You'll be all set with:
- A valid email address.
- A valid cell number.( I gave my landline)
- A valid credit card number( a virtual visa worked fine).

_NOTE:_ Till now (a couple of days) nothing has been charged from my credit card. Important is to select *Always free eligible* badge. There is a *Deploy a WordPress website*, but without this badge.

 It has been tested under Ubuntu 20, but should work for CentOS, Fedora or Red Hat Enterprise Linux.
-------------------------------------------------------------
# Step 0:
#### Creating an Instance

Follow the [Oracle docs.](https://docs.oracle.com/en-us/iaas/Content/Compute/Tasks/launchinginstance.htm)

# STEP 1:
#### Connect to Remote Server Without Password
At instance creation you saved the private key to your computer. Now open a terminal and login  with:      

    ssh root@your-server-ip

Now update and upgrade the list of packages

/* if Ubuntu*/

    sudo apt update
    sudo apt upgrade

/* if CentOS, Fedora or RHEL*/

    sudo yum update
    sudo yum upgrade

You're root, so pay attention.

# STEP 2:
#### Open the firewall
We have to allow added ports in the system firewall. Use this to allow an HTTP & HTTPS ports:

    sudo apt install firewalld
    sudo firewall-cmd --permanent --zone=public --add-port=80/tcp
    sudo firewall-cmd --permanent --zone=public --add-port=443/tcp

# STEP 3:
#### Refresh the firewall
Reload the firewall to make changes active:

    sudo firewall-cmd --reload

# STEP 4:
#### Check swap
Check your available memory and current swap space:

    free -m

# STEP 5:
#### _Optional_ - Allocate some swap memory
Now create a swap file and allocate space. Here I am using 1 GB but you can add more. 1024 is the size of the swap file in megabytes.

    sudo dd if=/dev/zero of=/mnt/swap.0 bs=1024 count=1048576
    sudo mkswap /mnt/swap.0
    sudo swapon /mnt/swap.0

Verify that the swap is available:

    sudo swapon --show  

In the case you cannot create swap, try:

    swapoff /mnt/swap.0
    mkswap /mnt/swap.0
    swapon /mnt/swap.0    



# STEP 6:
#### Troubleshoot issues
Install Bitnami dependencies

/* if  Ubuntu -> Missing library libtinfo.so.5*/

    sudo apt install libncurses5

/* if CentOS, Fedora or RHEL and Fedora -> Missing library libnsl.so.1

    sudo yum install ncurses-compat-libs
    sudo yum install libnsl


# STEP 7:
#### _Optional_ Perl

Install Perl. So the server got prepared to run the WordPress setup.

/* if Ubuntu*/

    sudo apt install perl

/* if CentOS/Fedora/RHEL*/

    sudo yum install perl perl-Data-Dumper
    
# STEP 8:
#### Download bitnami 
get the latest version of [Bitnami](https://bitnami.com/stack/wordpress/installer) at the official site - Remember to replace the current download link

    sudo wget https://bitnami.com/redirect/to/2226262/bitnami-wordpress-6.1-0-linux-x64-installer.run
    
# STEP 9:
Provide execute permission on the installer package using this command.

    sudo chmod 744 bitnami-wordpress-6.1-0-linux-x64-installer.run

# STEP 10:
execute the installer.

    sudo ./bitnami-wordpress-6.1-0-linux-x64-installer.run


# Bonus----------

# 1 Remove Bitnami Welcome page

    /opt/wordpress-6.1-0/apps/wordpress/bnconfig --appurl /

# 2 Remove Bitnami banner at the bottom right corner

    sudo /opt/wordpress-6.1-0/apps/wordpress/bnconfig --disable_banner 1

# 3 restart the apache server for changes to take effect

    sudo /opt/wordpress-6.1-0/ctlscript.sh restart apache


# Remember to point your domain to an IP
Now all you need is to point your domain to your machine public ip by adding two DNS records:

# “A” record
@ public IP

# CNAME record
www @

# Generate a Free SSL certificate, cron job renew and force redirection from HTTP to HTTPS, www to non-www

    sudo /opt/wordpress-6.1-0/bncert-tool

## That's all folks. Now go to an audio streamer and search for: 
### *Can't Take My Eyes Off You*  
### and sing along: 
# You're just too good to be true ...
