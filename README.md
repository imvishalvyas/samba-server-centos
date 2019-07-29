# sanba-server-centos


## Install samba package using yum :
```
yum -y install samba*
```

Create a directory which you want to share and set full permission on that directory.
```
mkdir -p /home/samba
```
```
chmod -R 0777 /home/samba
```
```
chmod -R 0777 /home/samba/
```

Now open samba configuration file and make some changes for share anonymous (password less directory sharing).

`vim /etc/samba/smb.conf`

Make changes in file which marked in bold.
```
[global]
        workgroup = workgroup
        security = user 
        passdb backend = tdbsam
        map to guest = Bad User
        printing = cups
        printcap name = cups
        load printers = yes
        cups options = raw

[guest share]
path = /home/samba
writable = yes
browsable = yes
guest ok = yes
guest only = yes
create mode = 0777
directory mode = 0777
```


Now restart the samba service and make them enable on reboot.
```
# systemctl start smb
# systemctl start nmb
# systemctl enable smb
# systemctl enable nmb
```

We can check samba configuration syntax error using testparm :
```
# testparm
```

[root@imcl samba]# testparm
```
Load smb config files from /etc/samba/smb.conf
rlimit_max: increasing rlimit_max (1024) to minimum Windows limit (16384)
Processing section "[guest share]"
Processing section "[homes]"
Processing section "[printers]"
Processing section "[print$]"
Loaded services file OK.
Server role: ROLE_STANDALONE

Press enter to see a dump of your service definitions

# Global parameters
[global]
        printcap name = cups
        map to guest = Bad User
        security = USER
        idmap config * : backend = tdb
        cups options = raw


[guest share]
        path = /home/samba
        create mask = 0777
        directory mask = 0777
        guest ok = Yes
        guest only = Yes
        read only = No

```


Block files with certain pattern like extensions to prevent dissemination of viruses or users from wasting space with certain files:

```
[guest share]
path = /home/samba
writable = yes
browsable = yes
guest ok = yes
guest only = yes
create mode = 0777
directory mode = 0777
Veto files = /*.exe/*.com/*.dll/*.bat/*.vbs/*.tmp/*.mp3/*.avi/*.mp4/*.wmv/*.wma/
```


Allow samba service and port from firewall.
```
# firewall-cmd --permanent --add-port=137/tcp
# firewall-cmd --permanent --add-port=138/tcp
# firewall-cmd --permanent --add-port=139/tcp
# firewall-cmd --permanent --add-port=445/tcp
# firewall-cmd --permanent --add-port=901/tcp
# firewall-cmd --reload
```
Now you can acces that shared folder from windows machine without password and upload any files and folders.

