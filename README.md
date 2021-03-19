# playbook_lesson

##1. Get CPU info
`ansible@ansible-ctl:~/playbook_lesson$ ansible-playbook lesson10.yml -i inventory -l db --tags l10_get_hw_tag`
```
PLAY [db] *****************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************
ok: [ansible3]

TASK [l10_get_hw : Show hostname] *****************************************************************************************
ok: [ansible3] => {
    "ansible_hostname": "ansible3"
}

TASK [l10_get_hw : HW info] ***********************************************************************************************
ok: [ansible3] => {
    "msg": [
        "CPU Core: 1",
        ""
    ]
}

PLAY RECAP ****************************************************************************************************************
ansible3                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

##2. Create user/install nginx


`ansible@ansible-ctl:~/playbook_lesson$ ansible-playbook lesson10.yml -i inventory -l db --tags l10_create_user_tag`
```
PLAY [db] *****************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************
ok: [ansible3]

TASK [l10_create_user : Add user "testuser" to remote server] *************************************************************
[WARNING]: The input password appears not to have been hashed. The 'password' argument must be encrypted for this module
to work properly.
changed: [ansible3]

PLAY RECAP ****************************************************************************************************************
ansible3                   : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
`ansible@ansible-ctl:~/playbook_lesson$ ansible-playbook lesson10.yml -i inventory -l db --tags l10_install_nginx_tag`
```
PLAY [db] *****************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************
ok: [ansible3]

TASK [l10_install_nginx : Install Nginx] **********************************************************************************
changed: [ansible3]

RUNNING HANDLER [l10_install_nginx : nginx systemd] ***********************************************************************
ok: [ansible3]

PLAY RECAP ****************************************************************************************************************
ansible3                   : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```

##3. Copy file to remote server
`ansible@ansible-ctl:~/playbook_lesson$ ansible-playbook lesson10.yml -i inventory -l db --tags l10_copy_file_tag`
```
PLAY [db] *****************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************
ok: [ansible3]

TASK [l10_copy_file : Copy file] ******************************************************************************************
changed: [ansible3]

PLAY RECAP ****************************************************************************************************************
ansible3                   : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

##4. Delete file, user. Uninstall nginx
`ansible@ansible-ctl:~/playbook_lesson$ ansible-playbook lesson10.yml -i inventory -l db --tags l10_delete_all_tag`
```
PLAY [db] *****************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************
ok: [ansible3]

TASK [l10_delete_all : Remove file from remote server] ********************************************************************
changed: [ansible3]

TASK [l10_delete_all : Delete user "testuser"] ****************************************************************************
changed: [ansible3]

TASK [l10_delete_all : Delete Nginx] **************************************************************************************
changed: [ansible3]

PLAY RECAP ****************************************************************************************************************
ansible3                   : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

##5. Create Docker containers
`ansible@ansible-ctl:~/playbook_lesson$ ansible-playbook lesson10.yml -i inventory -l db --tags l10_create_docker_tag`
```
PLAY [db] *****************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************
ok: [ansible3]

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

##6. Install proftpd and add "Hello" message
`ansible@ansible-ctl:~/playbook_lesson$ ansible-playbook lesson10.yml -i inventory -l db --tags l10_install_ftp_tag`
```
PLAY [db] *****************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************
ok: [ansible3]

TASK [l10_install_ftp : Install proftpd] **********************************************************************************
changed: [ansible3]

TASK [l10_install_ftp : Create /srv/ftp dir] ******************************************************************************
changed: [ansible3]

TASK [l10_install_ftp : copy] *********************************************************************************************
changed: [ansible3]

TASK [l10_install_ftp : copy] *********************************************************************************************
changed: [ansible3]

RUNNING HANDLER [l10_install_ftp : Restart proftpd] ***********************************************************************
changed: [ansible3]

PLAY RECAP ****************************************************************************************************************
ansible3                   : ok=6    changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## CHECK FTP
```
ansible@ansible-ctl:~/playbook_lesson$ ftp 10.128.0.22
Connected to 10.128.0.22.
220 ProFTPD Server (Debian) [::ffff:10.128.0.22]
Name (10.128.0.22:pavel): ansible
331 Password required for ansible
Password:
230-****Lesson 10 FTP****
230 User ansible logged in
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>
```

##7. Check http service
`ansible@ansible-ctl:~/playbook_lesson$ ansible-playbook lesson10.yml -i inventory -l db --tags l10_check_http_service_tag`
```
PLAY [db] *****************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************
ok: [ansible3]

TASK [l10_check_http_service : Check nginx status] ************************************************************************
fatal: [ansible3]: FAILED! => {"changed": true, "cmd": ["systemctl", "status", "nginx"], "delta": "0:00:00.010378", "end": "2021-03-19 08:27:47.696203", "msg": "non-zero return code", "rc": 3, "start": "2021-03-19 08:27:47.685825", "stderr": "", "stderr_lines": [], "stdout": "● nginx.service\n     Loaded: masked (Reason: Unit nginx.service is masked.)\n     Active: inactive (dead) since Fri 2021-03-19 08:27:27 UTC; 20s ago\n   Main PID: 116483 (code=exited, status=0/SUCCESS)\n\nMar 19 08:26:26 ansible3 systemd[1]: Starting A high performance web server and a reverse proxy server...\nMar 19 08:26:27 ansible3 systemd[1]: Started A high performance web server and a reverse proxy server.\nMar 19 08:27:27 ansible3 systemd[1]: Stopping A high performance web server and a reverse proxy server...\nMar 19 08:27:27 ansible3 systemd[1]: nginx.service: Succeeded.\nMar 19 08:27:27 ansible3 systemd[1]: Stopped A high performance web server and a reverse proxy server.", "stdout_lines": ["● nginx.service", "     Loaded: masked (Reason: Unit nginx.service is masked.)", "     Active: inactive (dead) since Fri 2021-03-19 08:27:27 UTC; 20s ago", "   Main PID: 116483 (code=exited, status=0/SUCCESS)", "", "Mar 19 08:26:26 ansible3 systemd[1]: Starting A high performance web server and a reverse proxy server...", "Mar 19 08:26:27 ansible3 systemd[1]: Started A high performance web server and a reverse proxy server.", "Mar 19 08:27:27 ansible3 systemd[1]: Stopping A high performance web server and a reverse proxy server...", "Mar 19 08:27:27 ansible3 systemd[1]: nginx.service: Succeeded.", "Mar 19 08:27:27 ansible3 systemd[1]: Stopped A high performance web server and a reverse proxy server."]}
...ignoring

TASK [l10_check_http_service : Show nginx status] *************************************************************************
ok: [ansible3] => {
    "result": {
        "changed": true,
        "cmd": [
            "systemctl",
            "status",
            "nginx"
        ],
        "delta": "0:00:00.010378",
        "end": "2021-03-19 08:27:47.696203",
        "failed": true,
        "msg": "non-zero return code",
        "rc": 3,
        "start": "2021-03-19 08:27:47.685825",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "● nginx.service\n     Loaded: masked (Reason: Unit nginx.service is masked.)\n     Active: inactive (dead) since Fri 2021-03-19 08:27:27 UTC; 20s ago\n   Main PID: 116483 (code=exited, status=0/SUCCESS)\n\nMar 19 08:26:26 ansible3 systemd[1]: Starting A high performance web server and a reverse proxy server...\nMar 19 08:26:27 ansible3 systemd[1]: Started A high performance web server and a reverse proxy server.\nMar 19 08:27:27 ansible3 systemd[1]: Stopping A high performance web server and a reverse proxy server...\nMar 19 08:27:27 ansible3 systemd[1]: nginx.service: Succeeded.\nMar 19 08:27:27 ansible3 systemd[1]: Stopped A high performance web server and a reverse proxy server.",
        "stdout_lines": [
            "● nginx.service",
            "     Loaded: masked (Reason: Unit nginx.service is masked.)",
            "     Active: inactive (dead) since Fri 2021-03-19 08:27:27 UTC; 20s ago",
            "   Main PID: 116483 (code=exited, status=0/SUCCESS)",
            "",
            "Mar 19 08:26:26 ansible3 systemd[1]: Starting A high performance web server and a reverse proxy server...",
            "Mar 19 08:26:27 ansible3 systemd[1]: Started A high performance web server and a reverse proxy server.",
            "Mar 19 08:27:27 ansible3 systemd[1]: Stopping A high performance web server and a reverse proxy server...",
            "Mar 19 08:27:27 ansible3 systemd[1]: nginx.service: Succeeded.",
            "Mar 19 08:27:27 ansible3 systemd[1]: Stopped A high performance web server and a reverse proxy server."
        ]
    }
}

PLAY RECAP ****************************************************************************************************************
ansible3                   : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=1
```
