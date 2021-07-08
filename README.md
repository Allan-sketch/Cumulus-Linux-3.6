# Cumulus-Linux-3.6
Step by step installation of cumulus linux
#Adding and Updating Packages
You use the Advanced Packaging Tool (apt) to manage additional applications (in the form of packages) and to install the latest updates.

Before running any apt-get commands or after changing the /etc/apt/sources.list file, you need to run apt-get update.
#Warning:-

#Network Disruptions When Updating/Upgrading

The apt-get upgrade and apt-get install commands cause disruptions to network services:

The apt-get upgrade command might result in services being restarted or stopped as part of the upgrade process.
The apt-get install command might disrupt core services by changing core service dependency packages.
In some cases, installing new packages with apt-get install might also upgrade additional existing packages due to dependencies. To view the additional packages that will be installed and/or upgraded before installing, run apt-get install --dry-run.




#As 
Cumulus Networks recommends you use the -E option with sudo whenever you run any apt-get command. This option preserves your environment variables (such as HTTP proxies) before you install new packages or upgrade your distribution.


Updating-->


cumulus@switch:~$ dpkg -l | grep {name of package}
