# Ansible Modules

## Finding modules

1. ansible-doc -l => List all available modules
2. ansible-doc -s _module_name_=> Snippet of sample YML for the module
3. ansible-doc _module_name_ => Man-page for the module

### Copy Module

* Copy a file from the ACS to the remote system
* Make a backup if the file exists.
* Remote validataion

### Fetch Module

* Pull files from a remote system

### Apt Module

* Work with APT packages.

### Yum Module

* Work with yum to install RPMs, etc.

### Service Module

* Stop, start, disable, enable services.



## Demo Time:

1. Browse Module documentation

2. Install Web Server (Yum Module)

   1. ansible webservers -i inventory -m yum -a "name=httpd state=present" --sudo
      ```
      web1 | success >> {

          "changed": true,
          "msg": "",
          "rc": 0,
          "results": [
              "Loaded plugins: fastestmirror\nSetting up Install Process\nLoading mirror speeds from cached hostfile\n * base: mirror.millry.co\n * epel: mirrors.syringanetworks.net\n * extras: centos.aol.com\n * updates: ftpmirror.your.org\nResolving Dependencies\n--> Running transaction check\n---> Package httpd.x86_64 0:2.2.15-54.el6.centos will be installed\n--> Processing Dependency: httpd-tools = 2.2.15-54.el6.centos for package: httpd-2.2.15-54.el6.centos.x86_64\n--> Processing Dependency: apr-util-ldap for package: httpd-2.2.15-54.el6.centos.x86_64\n--> Processing Dependency: /etc/mime.types for package: httpd-2.2.15-54.el6.centos.x86_64\n--> Processing Dependency: libaprutil-1.so.0()(64bit) for package: httpd-2.2.15-54.el6.centos.x86_64\n--> Processing Dependency: libapr-1.so.0()(64bit) for package: httpd-2.2.15-54.el6.centos.x86_64\n--> Running transaction check\n---> Package apr.x86_64 0:1.3.9-5.el6_2 will be installed\n---> Package apr-util.x86_64 0:1.3.9-3.el6_0.1 will be installed\n---> Package apr-util-ldap.x86_64 0:1.3.9-3.el6_0.1 will be installed\n---> Package httpd-tools.x86_64 0:2.2.15-54.el6.centos will be installed\n---> Package mailcap.noarch 0:2.1.31-2.el6 will be installed\n--> Finished Dependency Resolution\n\nDependencies Resolved\n\n================================================================================\n Package            Arch        Version                      Repository    Size\n================================================================================\nInstalling:\n httpd              x86_64      2.2.15-54.el6.centos         updates      833 k\nInstalling for dependencies:\n apr                x86_64      1.3.9-5.el6_2                base         123 k\n apr-util           x86_64      1.3.9-3.el6_0.1              base          87 k\n apr-util-ldap      x86_64      1.3.9-3.el6_0.1              base          15 k\n httpd-tools        x86_64      2.2.15-54.el6.centos         updates       79 k\n mailcap            noarch      2.1.31-2.el6                 base          27 k\n\nTransaction Summary\n================================================================================\nInstall       6 Package(s)\n\nTotal download size: 1.1 M\nInstalled size: 3.7 M\nDownloading Packages:\n--------------------------------------------------------------------------------\nTotal                                           728 kB/s | 1.1 MB     00:01     \nRunning rpm_check_debug\nRunning Transaction Test\nTransaction Test Succeeded\nRunning Transaction\n\r  Installing : apr-1.3.9-5.el6_2.x86_64                                     1/6 \n\r  Installing : apr-util-1.3.9-3.el6_0.1.x86_64                              2/6 \n\r  Installing : apr-util-ldap-1.3.9-3.el6_0.1.x86_64                         3/6 \n\r  Installing : httpd-tools-2.2.15-54.el6.centos.x86_64                      4/6 \n\r  Installing : mailcap-2.1.31-2.el6.noarch                                  5/6 \n\r  Installing : httpd-2.2.15-54.el6.centos.x86_64                            6/6 \n\r  Verifying  : apr-util-ldap-1.3.9-3.el6_0.1.x86_64                         1/6 \n\r  Verifying  : apr-1.3.9-5.el6_2.x86_64                                     2/6 \n\r  Verifying  : httpd-tools-2.2.15-54.el6.centos.x86_64                      3/6 \n\r  Verifying  : mailcap-2.1.31-2.el6.noarch                                  4/6 \n\r  Verifying  : httpd-2.2.15-54.el6.centos.x86_64                            5/6 \n\r  Verifying  : apr-util-1.3.9-3.el6_0.1.x86_64                              6/6 \n\nInstalled:\n  httpd.x86_64 0:2.2.15-54.el6.centos                                           \n\nDependency Installed:\n  apr.x86_64 0:1.3.9-5.el6_2                                                    \n  apr-util.x86_64 0:1.3.9-3.el6_0.1                                             \n  apr-util-ldap.x86_64 0:1.3.9-3.el6_0.1                                        \n  httpd-tools.x86_64 0:2.2.15-54.el6.centos                                     \n  mailcap.noarch 0:2.1.31-2.el6                                                 \n\nComplete!\n"
          ]
      }
      ```

3. Start Web Server (Service Module)

   ```
   ansible web1 -i inventory -m service -a &quot;name=httpd state=started enabled=yes&quot; --sudo

   web1 | success >> {

       "changed": true,
       "enabled": true,
       "name": "httpd",
       "state": "started"
   }
   ```

4. Install DB Server (Yum Module)

   ```
   ansible db1 -i inventory -m yum -a "name=mysql-server state=present"  --sudo
   ```

5. Start DB Server (Yum Module)

   ```
   ansible db -i inventory -m service -a "name=mysqld state=started enabled=yes" --sudo
   ```

   1. Try the web page: http://192.169.30.20

6. Stop Firewalls (Service Module)

   ```
   ansible web:db -i inventory -m service -a "name=iptables state=stopped enabled=false" --sudo
   ```

   1. Try the web page again: http://192.168.30.20
   2. Note the `web:db` host identification. That uses "Host/Group Target Patterns".
      1. `:` is the or pattern ( web or db => web:db)
      2. `!`is the not pattern (not db => !db)
      3. `*`is the wildcard pattern (web*.ex.com)
      4. `~regex` is the regex pattern ( ~web[0-0])
      5. `:&`is the and pattern ( This is an intersection of 2 sets.) ( All web servers that are prod and not python 2 => webservers:&prod:!python3)

### Setup Module

Used to gather facts about remote systems.

1. ansible web1 -i inventory -m setup 
   1. -a "filter=anisble_eth*"
   2. -a "filter=ansible_mounts"
   3. Very similar to Ohai, there are actually facts labeled Ohai as well
2. ansible all -i inventory -m setup —tree ./setup