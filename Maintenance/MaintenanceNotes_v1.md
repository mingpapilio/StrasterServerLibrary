# Straster servers maintenance document

| Field | Details |
|---|---|
| Author | Ming Liu|
| Last updated | 11 Aug 2026 |
| Version | 1.0.2 |
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
sudo /usr/local/sbin/assign_lab_group.sh $USER $GROUP
```

## 2.2 FosterStorage
```bash
## Connect to FosterStorage
ssh ming@192.168.50.10
## Add a user
sudo add_storage_user.sh $USER
```

### Set up microscope computer connections

# 3. Set up microscope computer connections

Configure the FosterStorage-facing Ethernet adapter with subnet mask `255.255.255.0`; leave gateway and DNS blank.

| Computer | Computer IP | FosterStorage IP |
|---|---|---|
| Petunia | `192.168.51.20` | `192.168.51.10` |
| Trillion | `192.168.52.20` | `192.168.52.10` |
| 42 | `192.168.53.20` | `192.168.53.10` |
| Seshat | `192.168.54.20` | `192.168.54.10` |

Create two desktop shortcuts on each computer, replacing `X` with its subnet number.

**Open FosterStorage:**

~~~text
\\192.168.X.10\FosterStorage
~~~

**Logout FosterStorage:**

~~~text
cmd.exe /c "net use \\192.168.X.10\FosterStorage /delete /y"
~~~

The shortcuts do not store passwords. Samba sessions end after using the logout shortcut or rebooting/logging out of Windows. Each computer must use its corresponding FosterStorage IP.

# 4. View FosterStorage authentication records

~~~bash
sudo grep -Rh '^  Auth:' /var/log/samba/log.* | perl -ne '
BEGIN {
    %mon = (Jan=>"01",Feb=>"02",Mar=>"03",Apr=>"04",May=>"05",Jun=>"06",
            Jul=>"07",Aug=>"08",Sep=>"09",Oct=>"10",Nov=>"11",Dec=>"12");
    printf "%-20s %-12s %-16s %-20s %s\n",
           "TIME (UTC)", "USER", "STATUS", "WORKSTATION", "CLIENT IP";
}
if (/user \[[^\]]+\]\\\[([^\]]+)\].*?at \[\w+,\s+(\d{1,2})\s+(\w+)\s+(\d{4})\s+(\d{2}:\d{2}:\d{2})(?:\.\d+)?\s+UTC\].*?status \[([^\]]+)\].*?workstation \[([^\]]+)\].*?remote host \[ipv4:([^:\]]+):/) {
    printf "%04d-%s-%02d %-8s %-12s %-16s %-20s %s\n",
           $4, $mon{$3}, $2, $5, $1, $6, $7, $8;
}'
~~~

This displays the authentication time, Samba username, status, workstation and client IP. `NT_STATUS_OK` indicates successful authentication.

# 5. Source code for the welcome message
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

# 6. Upgrade and additional notes

- The best RAM model: DDR5 ECC RDIMM (probably 4x128GB would be the best value over 8 sockets).
- To check how many process a user can have: `cat /etc/security/limits.d/*.conf | grep -i nproc` (currently set to 2048 and 4096 for soft and hard limits)

