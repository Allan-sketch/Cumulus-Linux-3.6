# Cumulus-Linux-3.6
Step by step installation of cumulus linux
## Adding and Updating Packages
You use the Advanced Packaging Tool (apt) to manage additional applications (in the form of packages) and to install the latest updates.

Before running any 
````
apt-get
````

 commands or after changing the /etc/apt/sources.list file, you need to run
````
 apt-get update
````

## Warning:-

## Network Disruptions When Updating/Upgrading

The
````
 apt-get upgrade
````
 and 
````
apt-get install
````
 commands cause disruptions to network services:

The apt-get upgrade command might result in services being restarted or stopped as part of the upgrade process.
The apt-get install command might disrupt core services by changing core service dependency packages.
In some cases, installing new packages with apt-get install might also upgrade additional existing packages due to dependencies. To view the additional packages that will be installed and/or upgraded before installing, run
````
 apt-get install --dry-run.
````



## As 
Cumulus Networks recommends you use the -E option with sudo whenever you run any apt-get command. This option preserves your environment variables (such as HTTP proxies) before you install new packages or upgrade your distribution.


## Updating-->

````
cumulus@switch:~$ dpkg -l | grep {name of package}
````

If the package is installed already, ensure it is the version you need. If the package is an older version, update the package from the Cumulus Linux repository:

````
cumulus@switch:~$ sudo -E apt-get upgrade
````
If the package is not already on the system, add it by running 
````
apt-get install
````
. This retrieves the package from the Cumulus Linux repository and installs it on your system together with any other packages on which this package might depend.

For example, the following adds the package 

````
tcpreplay
````

 to the system:

````
cumulus@switch:~$ sudo -E apt-get install tcpreplay
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following NEW packages will be installed:
tcpreplay
0 upgraded, 1 newly installed, 0 to remove and 1 not upgraded.
Need to get 436 kB of archives.
After this operation, 1008 kB of additional disk space will be used.
Get:1 https://repo.cumulusnetworks.com/ CumulusLinux-1.5/main tcpreplay amd64 4.6.2-5+deb8u1 [436 kB]
Fetched 436 kB in 0s (1501 kB/s)
Selecting previously unselected package tcpreplay.
(Reading database ... 15930 files and directories currently installed.)
Unpacking tcpreplay (from .../tcpreplay_4.6.2-5+deb8u1_amd64.deb) ...
Processing triggers for man-db ...
Setting up tcpreplay (4.6.2-5+deb8u1) ...
cumulus@switch:~$ 
````

## Listing Installed Packages

The APT cache contains information about all the packages available on the repository. To see which packages are actually installed on your system, use dpkg. The following example lists all the package names on the system that contain tcp
:

````
cumulus@switch:~$ dpkg -l \*tcp\*
Desired=Unknown/Install/Remove/Purge/Hold
| Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
||/ Name                          Version             Architecture        Description
+++-=============================-===================-===================-===============================================================
un  tcpd                          <none>              <none>              (no description available)
ii  tcpdump                       4.6.2-5+deb8u1      amd64               command-line network traffic analyzer
cumulus@switch:~$
````

## Upgrading to Newer Versions of Installed Packages


## Upgrading a Single Package

You can upgrade a single package by running 
````
apt-get install
````
. Perform an update first so that the APT cache is populated with the latest package information.

To see if a package needs to be upgraded, run the 
````
apt-cache show <pkgname>
````
 command to show the latest version number of the package. Use 

````
dpkg -l <pkgname>
````
 to show the version number of the installed package.

## Upgrading All Packages

You can update all packages on the system by running 

````
apt-get update
````

, then 

````
apt-get upgrade
````
. This upgrades all installed versions with their latest versions but does not install any new packages.
## Adding Packages from Another Repository

As shipped, Cumulus Linux searches the Cumulus Linux repository for available packages. You can add additional repositories to search by adding them to the list of sources that 
````
apt-get
```` 

consults. See 


````
man sources.list
````
 for more information.


## TIP
````
Cumulus Networks has added features or made bug fixes to certain packages; you must not replace these packages with versions from other repositories. Cumulus Linux is configured to ensure that the packages from the Cumulus Linux repository are always preferred over packages from other repositories.
````

If you want to install packages that are not in the Cumulus Linux repository, the procedure is the same as above, but with one additional step.

## Note

````
Packages that are not part of the Cumulus Linux Repository are not typically tested and might not be supported by Cumulus Linux Technical Support.
````

Installing packages outside of the Cumulus Linux repository requires the use of apt-get; however, depending on the package, you can use 

````
easy-install
````
 and other commands.

1.Run the dpkg command to ensure that the package is not already installed on the system:
````
cumulus@switch:~$ dpkg -l | grep {name of package}
````

2.If the package is installed already, ensure it is the version you need. If it is an older version, update the package from the Cumulus Linux repository:

````
cumulus@switch:~$ sudo -E apt-get update
cumulus@switch:~$ sudo -E apt-get install {name of package}
cumulus@switch:~$ sudo -E apt-get upgrade
````

3.If the package is not on the system, the package source location is most likely not in the /etc/apt/sources.list file. If the source for the new package is not in sources.list, edit and add the appropriate source to the file. For example, add the following if you want a package from the Debian repository that is not in the Cumulus Linux repository:

````
deb http://http.us.debian.org/debian jessie main
deb http://security.debian.org/ jessie/updates main
````
Otherwise, the repository might be listed in /etc/apt/sources.list but is commented out, as can be the case with the early-access repository:

````
#deb http://repo3.cumulusnetworks.com/repo CumulusLinux-3-early-access cumulus
````

To uncomment the repository, remove the # at the start of the line, then save the file:

````
deb http://repo3.cumulusnetworks.com/repo CumulusLinux-3-early-access cumulus
````

