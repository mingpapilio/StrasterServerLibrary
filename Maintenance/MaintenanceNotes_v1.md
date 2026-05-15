# Straster servers maintenance document

| Field | Details |
|---|---|
| Author | Ming Liu|
| Last updated | 14 May 2026 |
| Version | 1.0 |
| Scope | Notes for admin to manage servers |

<br><br>

# 1. Regular maintenance
## 1.1 System updates
```bash
# Standard commands
sudo apt update
sudo apt upgrade
# In case of missing anything, you can also try 
sudo apt-get update
sudo apt-get upgrade
# Clean up Jupyter trash
# Source: https://stackoverflow.com/questions/63498851/how-to-free-up-disk-space-when-deleting-files-from-jupyter-notebook
sudo rm -rf $HOME/.local/share/Trash/files
# Reboot the machine
sudo reboot
```
## 1.2 Update system messages
```bash
# Modify general message
sudo nano /etc/motd
# 'ctrl+ x' to exit, then press 'y' to save changes
```
## 1.3 Emergency
```{bash eval=F}
# Kill a process with sudo access
sudo pkill PROCESS_ID
```
# 2. Adding users
## 2.1 StrasterTower
```bash
sudo adduser $USER
# Assign people to user groups
sudo /usr/local/sbin/assign_lab_group.sh ming foster
```

## 2.2 FosterStorage
```bash
## Connect to FosterStorage
ssh ming@192.168.50.10
## Add a user
sudo add_storage_user.sh $USER
```

# 3. Source code for the welcome message
```bash
# Dynamic storage display
sudo nano /etc/update-motd.d/99-custom
## Inside the file
#!/bin/sh
echo "SYSTEM DISK USAGE"
printf "%-20s %-7s %-7s %-5s\n" "Location" "Size" "Used" "Use%"
df -h --output=target,size,used,pcent / /drives/4tb /drives/12tb_foster /drives/12tb_stracy | tail -n +2 | \
while read -r target size used pcent; do
    printf "%-20s %-7s %-7s %-5s\n" "$target" "$size" "$used" "$pcent"
done
echo
## Check displays
sudo run-parts /etc/update-motd.d/

# Modify general message
sudo nano /etc/motd

--------------------------------------------------------
|                                                      |
|           Welcome to Straster Tower Server!!         |
|                                                      |
|  A few reminders:                                    |
|  - Please always change directory by                 |
|                cd /drives/4tb/username               |
|  - DO NOT install any R packages yourself (contact   |
|    Ming or Laura and we will do it for you)          |
|  - Contact us whenever you are adding large files    |
|    (>0.5 TB)                                         |
|  - Only use 30 threads if your task lasts > 1 day    |
|                                                      |
|               Next maintenance session:              |
|                                                      |
|               XX:XX - XX:XX PM  DD  MMM              |
|                                                      |
--------------------------------------------------------
```

