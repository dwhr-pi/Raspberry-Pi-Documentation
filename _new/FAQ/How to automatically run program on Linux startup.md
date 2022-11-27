# How to automatically run program on Linux startup
<!-- Quelle: https://www.simplified.guide/linux/automatically-run-program-on-startup>
<!-- -->
Linux startup is divided into a few stages. You can set any program to start automatically at any stage, whether it could be a single command, a chain of commands, or an executable shell script. However, there could be some differences in startup procedure between different Linux distributions and versions.  
!(Linux-CLI)[img\cli.png]
Modern Linux will first boot into systemd while older versions of Linux use System V init. Regardless, eventually, they will run cron and rc.local before loading the desktop environment, such as GNOME or KDE. On the other hand, server-based Linux distributions will not load the desktop environment but will immediately provide a login prompt at the console and then run the default shell, such as Bash, after the user logs in.  

Methods to automatically run program on Linux startup:
    * Manage using systemd
    * Create a cron job
    * Run using rc.local
    * Run on GNOME startup
    * Run on KDE startup
    * Run on new Bash session

## Automatically run program on Linux startup via systemd
systemd is the standard system and service manager in modern Linux. It is responsible for executing and managing programs during Linux startup, among many other things. Compatible programs will provide service unit files used by systemd to manage the program's execution.  
You can configure systemd to run programs automatically during Linux startup following these steps:  

    1. Check if service unit for your program exists (optional).

       ```
       $ sudo systemctl list-unit-files --type=service
       [sudo] password for user:
       UNIT FILE                              STATE
       accounts-daemon.service                enabled
       apparmor.service                       enabled
       apport-autoreport.service              static
       apport-forward@.service                static
       apport.service                         generated
       apt-daily-upgrade.service              static
       apt-daily.service                      static
       atd.service                            enabled
       autovt@.service                        enabled
       blk-availability.service               enabled
       bootlogd.service                       masked
       bootlogs.service                       masked
       bootmisc.service                       masked
       checkfs.service                        masked
       checkroot-bootclean.service            masked
       checkroot.service                      masked
       cloud-config.service                   enabled
       cloud-final.service                    enabled
       cloud-init-local.service               enabled
       cloud-init.service                     enabled
       console-getty.service                  disabled
       ##### snipped #####
       ```
       You'll have to create your own service unit if it's a custom program or if your program doesn't come with one during installation
Related: Creating and modifying systemd unit files
    2. Check if service unit enabled (optional).

       ```
       $ sudo systemctl is-enabled mysql
       disabled
       ```

       enabled service unit is executed during boot
    3. Enable service unit that you want to execute during startup.
       ```
       $ sudo systemctl enable mysql
       Synchronizing state of mysql.service with SysV service script with /lib/systemd/systemd-sysv-install.
       Executing: /lib/systemd/systemd-sysv-install enable mysql
       Created symlink /etc/systemd/system/multi-user.target.wants/mysql.service ? /lib/systemd/system/mysql.service.
       ```
    4. Check if service unit is enabled to confirm (optional).
       ```
       $ sudo systemctl is-enabled mysql
       enabled
       ```

## Automatically run program on Linux startup via cron
cron is a daemon to execute scheduled commands. The commands are stored in the cron job table or crontab and are unique for each user in the system. It's started during system boot either by systemd or System V init, and you can schedule your job or program to be executed right during the system boot itself by following these steps:
    1. Open the default crontab editor.
       ```
       $ crontab -e
       ```

       You're required to select an editor for the crontab if this is the first time the user uses the command.
       ```
       $ crontab -e
       no crontab for user - using an empty one
       
       Select an editor.  To change later, run 'select-editor'.
         1. /bin/nano        <---- easiest
         2. /usr/bin/vim.basic
         3. /bin/ed
       
       Choose 1-3 [1]:
       ```
       A crontab will be created for the user running the command and will be executed using the privileges of the user. If you need your program to run as the root user, run crontab -e as the root user itself.
    2. Add a line starting with @reboot.
       ```
       # m h  dom mon dow   command
       @reboot
       ```
       @reboot defines the job to be executed during system boot.
    3. Insert the command to start your program after the @reboot.
       ```
       @reboot /sbin/ip addr | grep inet\ | tail -n1 | awk '{ print $2 }' > /etc/issue && echo "" >> /etc/issue
       ```
       Use full path for your programs when possible and write your commands in a single line.
    4. Save the file to install it to the crontab.
       ```
       $ crontab -e
       crontab: installing new crontab
       $ 
       ```
       The file is saved in /var/spool/crontab/<username>
    5. Check if crontab is properly configured (optional).
       ```
       $ crontab -l
       # m h  dom mon dow   command
       @reboot /sbin/ip addr | grep inet\ | tail -n1 | awk '{ print $2 }' > /etc/issue && echo "" >> /etc/issue
       ```

## Automatically run program on Linux startup via rc.local
rc.local is a legacy from the System V init system. It is the last script to be executed before proceeding to a login screen for the desktop environment or a login prompt at the terminal. It's usually a Bash shell script, and you can run anything from the script.  
You can configure your rc.local script following these steps:
    1. Open or create /etc/rc.local file if it doesn't exist using your favourite editor as the root user.
       ```
       $ sudo vi /etc/rc.local
       ```
    2. Add placeholder code into the file.
       ```
       #!/bin/bash
       
       exit 0
       ```
       It must start with interpreter (/bin/bash) and ends with an exit code (0 is for success)
    3. Add command and logics to the file as necessary.
       ```
       #!/bin/bash
       
       /sbin/ip addr | grep inet\ | tail -n1 | awk '{ print $2 }' > /etc/issue
       echo "" >> /etc/issue
       
       exit 0
       ```
    4. Set the file to executable.
       ```
       $ sudo chmod a+x /etc/rc.local
       ```
       The file will be executed as the root user during system boot

## Automatically run program on GNOME startup
GNOME is the default desktop environment for Linux distributions such as Ubuntu and Red Hat. GNOME can be configured to run programs when a user logs in and can be configured by following the below article:
Related: How to automatically run program on GNOME startup

## Automatically run program on KDE startup
KDE is another popular desktop environment for Linux and is the default in Kubuntu and openSUSE. It can easily be configured to run programs when a user logs in as detailed in the following article:
Related: How to automatically run program on KDE startup

## Automatically run program on new Bash session
A new shell program will be spawned when you start your terminal session. Bash is the default shell for most Linuxdistributions, and when started, it will look for the following files in the particular order and executes them.

    1. /etc/profile
    2. ~/.bash_profile
    3. ~/.bash_login
    4. ~/.profile

These files contains commands and logics to set up proper environment variables and run required programs in Bash language. It's also configured to normally execute other files such as /etc/bashrc, /etc/bash.bashrc and ~/.bashrc.
You can edit any of these files to run your program when a Bash session is started. Below is a part of a typical ~/.bashrc file:

       ```
       PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\] \$ '
 
       PATH=/home/user/bin:$PATH
 
       export EDITOR=/usr/bin/vim
 
       alias ll="ls -l"
       ``` 

