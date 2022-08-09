# Ansible

this ansible setup is used for installing SSM onto hosts where it is not installed, either via snap or the default package manager.

Setup Instructions
1. Create a virtual environment
`python3 -m venv /path/to/venv`
2. Source the virtual environment
`source /path/to/venv/bin/active`
3. Install the requirements
`pip install requirements.txt`
4. Run the playbook
`ansible-playbook -i inventory/prod/inventory playbooks/ssm.yaml`

Output When SSM is Installed
```Ansible

PLAY [all] **********************************************************************************************************************************************************************

TASK [ssm_install : Check if amazon-ssm-agent is installed via snap] ************************************************************************************************************
changed: [13.58.95.143]

TASK [ssm_install : Define Snap Variable] ***************************************************************************************************************************************
ok: [13.58.95.143]

TASK [ssm_install : Check if amazon-ssm-agent is installed via package manager] *************************************************************************************************
ok: [13.58.95.143]

TASK [ssm_install : Define Package Manager Variable] ****************************************************************************************************************************
ok: [13.58.95.143]

TASK [ssm_install : snap debug information] *************************************************************************************************************************************
ok: [13.58.95.143] => {}

MSG:

SSM Installed Via Snap: False

TASK [ssm_install : package manager debug information] **************************************************************************************************************************
ok: [13.58.95.143] => {}

MSG:

SSM Installed Via Package Manager: True

TASK [ssm_install : Download and install amazon-ssm-agent deb package] **********************************************************************************************************
skipping: [13.58.95.143]

TASK [ssm_install : Enable Amazon SSM Agent service] ****************************************************************************************************************************
skipping: [13.58.95.143]

PLAY RECAP **********************************************************************************************************************************************************************
13.58.95.143               : ok=6    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
```

Uninstall `amazon-ssm-agent` on Ubuntu and re-run

```Bash
ubuntu@ip-172-31-10-157:~$ sudo apt-get remove amazon-ssm-agent
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following packages will be REMOVED:
  amazon-ssm-agent
0 upgraded, 0 newly installed, 1 to remove and 0 not upgraded.
After this operation, 0 B of additional disk space will be used.
Do you want to continue? [Y/n] Y
(Reading database ... 58053 files and directories currently installed.)
Removing amazon-ssm-agent (3.1.1634.0-1) ...
Stopping agent
```

Re-Run the script
```

PLAY [all] **********************************************************************************************************************************************************************

TASK [ssm_install : Check if amazon-ssm-agent is installed via snap] ************************************************************************************************************
changed: [13.58.95.143]

TASK [ssm_install : Define Snap Variable] ***************************************************************************************************************************************
ok: [13.58.95.143]

TASK [ssm_install : Check if amazon-ssm-agent is installed via package manager] *************************************************************************************************
fatal: [13.58.95.143]: FAILED! => {
    "changed": false,
    "rc": 139
}

MSG:

MODULE FAILURE
See stdout/stderr for the exact error


MODULE_STDERR:

Segmentation fault (core dumped)

...ignoring

TASK [ssm_install : Define Package Manager Variable] ****************************************************************************************************************************
ok: [13.58.95.143]

TASK [ssm_install : snap debug information] *************************************************************************************************************************************
ok: [13.58.95.143] => {}

MSG:

SSM Installed Via Snap: False

TASK [ssm_install : package manager debug information] **************************************************************************************************************************
ok: [13.58.95.143] => {}

MSG:

SSM Installed Via Package Manager: False

TASK [ssm_install : Download and install amazon-ssm-agent deb package] **********************************************************************************************************
changed: [13.58.95.143]

TASK [ssm_install : Enable Amazon SSM Agent service] ****************************************************************************************************************************
ok: [13.58.95.143]

PLAY RECAP **********************************************************************************************************************************************************************
13.58.95.143               : ok=8    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=1

(ansible)
```
