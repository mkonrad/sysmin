Introduction
============
pic - the postinstall adjustment tool is a basic shell script for making 
some minor adjustments to a Linux installation.  
The script changes are particularly useful if the system is a server 
running Java applications.  

This script was created to take care of some basic house keeping, creating 
a configuration backup directory /var/local/backup, changing some default 
settings in /etc/default/useradd and /etc/skel/.bashrc.

Additionally the script will create a service group and service account(1).
When creating the service group the script will change the group of
directory specified by the localdir variable, which is set to /opt/local 
by default, to the specified group and grants write access to the group. 
The service account will be made a member of the group and the service account
home will be set to /opt/local/&lt;user_name>. 

(1) Service accounts are not added to the /etc/subuid and /etc/subgid files
which prevents them from running containers. The pic script has been updated
to remove the -r flag from the useradd command.

Details
-------
### Useradd Defaults
Pic changes the default SHELL value in /etc/default/useradd. 

SHELL is set to /sbin/nologin

These changes effect how the useradd command is run to add new users. 

Syntax for adding a service account:<br />
__[root@machine ]#__ `/sbin/useradd -b /opt/local -m -G <group_name> <service_account>`

Syntax for adding a regular user account:<br />
__[root@machine ]#__ `/sbin/useradd -s /bin/bash -m -G <group_name> <user_name>`

Syntax for adding a service group:<br />
__[root@machine ]#__ `/usr/sbin/groupadd -r <group_name>` 


### Bashrc Changes
Pic adds `JAVA_HOME` to the /etc/skel/.bashrc file if Java has been set with
the /usr/sbin/alternativies tool.

Pic also adds `JAVA_HOME/bin` to the `PATH` variable.

Service Account Login
---------------------
With a service account SHELL set to /sbin/nologin, a user may not directly
login to the account, but instead sudos to the user account specifying a shell.

__[user@machine]$__ `sudo -u <service_account> -s /bin/bash`
__[service-account@machine]$__ `cd $HOME`


