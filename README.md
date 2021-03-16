# playbook_lesson

`ansible@ansible-ctl:~/playbook_lesson$ ansible-playbook lesson10.yml -i inventory`

##1.
```
PLAY [db] **********************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************
ok: [ansible3]

TASK [l10_get_hw : Show hostname] **********************************************************************************
ok: [ansible3] => {
    "ansible_hostname": "ansible3"
}

TASK [l10_get_hw : HW info] ****************************************************************************************
ok: [ansible3] => {
    "msg": [
        "CPU Core: 1",
        ""
    ]
}
```

##2.
```
TASK [l10_create_user : Add user "testuser" to remote server] ******************************************************
[WARNING]: The input password appears not to have been hashed. The 'password' argument must be encrypted for this
module to work properly.
changed: [ansible3]

TASK [l10_install_nginx : Install Nginx] ***************************************************************************
changed: [ansible3]
```

##3.
```
TASK [l10_copy_file : copy] ****************************************************************************************
changed: [ansible3]
```

##4.
```
TASK [l10_delete_all : Remove file from remote server] *************************************************************
changed: [ansible3]

TASK [l10_delete_all : Delete user "testuser"] *********************************************************************
changed: [ansible3]

TASK [l10_delete_all : Delete Nginx] *******************************************************************************
changed: [ansible3]
```

##5.
```
TASK [l10_create_docker : Install aptitude using apt] **************************************************************
changed: [ansible3]

TASK [l10_create_docker : Install required system packages] ********************************************************
changed: [ansible3] => (item=apt-transport-https)
ok: [ansible3] => (item=ca-certificates)
ok: [ansible3] => (item=curl)
changed: [ansible3] => (item=software-properties-common)
changed: [ansible3] => (item=python3-pip)
changed: [ansible3] => (item=virtualenv)
ok: [ansible3] => (item=python3-setuptools)
changed: [ansible3] => (item=python-setuptools)

TASK [l10_create_docker : Add Docker GPG apt Key] ******************************************************************
changed: [ansible3]

TASK [l10_create_docker : Add Docker Repository] *******************************************************************
changed: [ansible3]

TASK [l10_create_docker : Update apt and install docker-ce] ********************************************************
changed: [ansible3]

TASK [l10_create_docker : Install Docker Module for Python] ********************************************************
changed: [ansible3]

TASK [l10_create_docker : Pull default Docker image] ***************************************************************
changed: [ansible3]

TASK [l10_create_docker : pull an image] ***************************************************************************
changed: [ansible3]

TASK [l10_create_docker : Create default containers] ***************************************************************
changed: [ansible3] => (item=1)
changed: [ansible3] => (item=2)
changed: [ansible3] => (item=3)
```

##6.
```
TASK [l10_install_ftp : Install proftpd] ***************************************************************************
ok: [ansible3]

TASK [l10_install_ftp : copy] **************************************************************************************
changed: [ansible3]

TASK [l10_install_ftp : copy] **************************************************************************************
changed: [ansible3]
```

##7.
```
TASK [l10_check_http_service : Nginx status] ***********************************************************************
changed: [ansible3]

RUNNING HANDLER [l10_install_nginx : nginx systemd] ****************************************************************
ok: [ansible3]

PLAY RECAP *********************************************************************************************************
ansible3                   : ok=23   changed=18  
```

##Check FTP
```
ansible@ansible3:~$ ftp localhost
Connected to localhost.
220 ProFTPD Server (Debian) [::ffff:127.0.0.1]
Name (localhost:ansible):
331 Password required for ansible
Password:
230-****Lesson 10 FTP****
230 User ansible logged in
Remote system type is UNIX.
Using binary mode to transfer files.
```
