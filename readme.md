
Introduction
============
pic - the postinstall adjustment tool is a basic shell script for making 
some minor adjustments to a Linux installation.  
The script changes are particularly useful if the system is a server 
running Java applications.  

This script was created to take care of some basic house keeping, creating 
a configuration backup directory /var/local/backup, changing some default 
settings in /etc/default/useradd and /etc/skel/.bashrc.

Additionally the script will create a service group and service account.
When creating the service group, the script will change the group of /opt
to the specified group, and will grant write access to the group. The 
service account will be made a member of the group and the service account
home will be set to /opt/<user_name>. This script is a commitment to deploying
applications under the /opt filesystem. 

Recommend creating a generic services group and then adding each service 
account to this group.   

Details
-------
### Useradd Defaults
Pic changes the default SHELL value in /etc/default/useradd. 

SHELL is set to /usr/sbin/nologin


These changes effect how the useradd command is run to add new users. 

Syntax for adding a service account:
### [root@machine ]# /sbin/useradd -b /opt -mr -G <group_name> <service_account>

Syntax for adding a regular user account
### [root@machine ]# /sbin/useradd -s /bin/bash -m -G <user_name>

Syntax for adding a service group:
### [root@machine ]# /usr/sbin/groupadd -r <group_name> 


### Bashrc Changes
Pic adds JAVA_HOME to the /etc/skel/.bashrc file if Java has been set with
the /usr/sbin/alternativies tool.

Pic also adds JAVA_HOME/bin to the PATH variable.

Service Account Login
---------------------
With a service account SHELL set to /usr/sbin/nologin, a user may not directly
login to the account, but instead sudos to the user account specifying a shell.

### [user@machine]$ sudo -u <service_account> -s /bin/bash
### [<service_account@machine]$ cd $HOME 


