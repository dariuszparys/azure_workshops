# Proccesses and Run Levels

- You will be able to find/identify and kill processes.
- You will be able to change a process/services start/stop run level

## Processes

### Commands
Finding Process IDs (PIDs)
- ```top``` lists top running resources
- ```lsof``` list open files or ports
- ```pidof <process_name>``` e.g. ```pidof sshd``` finds the pids of the ssh daemon
- ```kill``` stop a process e.g. ```kill -9 3867``` immediately kills the Process with id 3867
- ```ps aux``` snapshot of all running processes

### Exercises
1. list current running processes
2. list all open files (process)
3. Filter and find all ```sshd``` PIDs

### Solutions
```bash
#1
ps aux

#2
lsof
```

## Service Run Levels

### Commands and files
Commands:
- ```sudo service <service_name> [start|stop|status]``` this command will start/stop or retrieve the status of a service e.g. apache2
- ```sudo update-rc.d``` this command is used to enable/disable a service from starting at X runtime level
    - syntax: ```sudo update-rc.d <service_name> [start|stop] <priority#> <runtimelevel 0-6> .```
    - example: ```sudo update-rc.d apache2 start 90 2 3 4 5 .``` adds start runtimes for levels 2-5 with priority 90 for apache2 if they do not exist already for those runlevels
        - if the runlevel script already exists in the particular ```/etc/rc[0-6].d``` folder you must remove it or change the file name:
            - example change kill to start in ```/etc/rc0.d/```: ```K90apache2``` to ```S90apache2```
    - Remove all scripts: ```sudo update-rc.d -f apache2 remove```

Files/Folders:
- ```/etc/init.d```
    - files in here are the start/stop/status scripts for services
    - these files are symlinked to the ```/etc/rc[0-6].d``` folders
    - these are the scripts that ```sudo service <service_name> start|stop|status``` uses to run the named service
- ```/etc/rc[0-6].d```
    - contains all the runlevel [0-6] symlinks
    - actually controls when a service starts/stop automatically when a system boots/shutsdown


## Crontab (cron jobs)
- crontab -l
- crontab -e
- crontab -r
- sudo cronttab -u username -l

### Crontab format
```
minute hour dayOfMonth month dayOfWeek
15 6 2 1 * /home/raykao/backup.sh
```
- dayOfMonth can be any single number 1-31, list of number 1,2,3,10,31 or range 1-10
- month can be a number 1-12 (list or range) OR name Jan,Feb (list of names)
- dayOfWeek number 0-7 (Sunday is 0 or 7), or name Mon,Tue etc. or in a list
