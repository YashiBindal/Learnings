# Linux Shell

- linux command line interface
- tool to directly interact with the OS
- each user in system have their own unique home directory
- home directory is represented by ~ symbol
- some commands need arguments, some don't
- 2 types of commands:
  - `Internal or Built-in`: echo, cd, pwd, set etc. Part of the shell itself, comes along with it. There are around 30 such commands
  - `External`: mv, date, uptime, cp, etc. These are binary program or scripts that come preinstalled with the distributions package manager or can be created or installed dby the user
- to Identify type of command use `type command`: <type echo>, <type mv>
- a command can have arguments, options and flags
- `absolute path` : location path starting from root directory eg. </home/yashi/asia>
- `relative path` : passing normal path
- to check shell being used : `echo $SHELL`
- to check home directory of user : `echo $HOME`
- bash shell supports auto-completion feature   
- use `$` to refer to environment variables
- to make variable available over subsequent reboots and login, stor the nev variable in /.profile or /.pam_environment files
- need to set path variables for external commands for them to run properly `export PATH=$PATH:/opt/obs/bin`
- update BASH PROMPT to user name using PS1 variable along with various flags
  <echo 'PS1="[\d]\u@\h:\w$"' >> ~/.profile>
- add variable using cmd to .profile file
  <echo 'export MY_VARIABLE="example_value"' >> ~/.profile>
- add command alias to /.profile
  <echo 'alias ll="ls -l"' >> ~/.profile>
  <echo 'alias up=uptime' >> /home/bob/.profile>
- Linux boot sequence
  - BIOs POST--> Boot loader(GRUB2) --> Kernel Initialization--> INIT Process(systemd)
  - 1. POST stands for power on self test: ensure that hardware devices attached are functioning correctly, if POST fails, that means system is inoperable and system will not proceed to next step
  - 2. bootloader is present in /boot 
  - use command ` ls -l /sbin/init` to check the type of INIT process