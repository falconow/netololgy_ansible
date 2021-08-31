> Выполнил подготовительную часть задания. 
 ```buildoutcfg
root@vagrant:~/netololgy_ansible# ansible --version
ansible [core 2.11.4] 
  config file = None
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /root/.local/lib/python3.8/site-packages/ansible
  ansible collection location = /root/.ansible/collections:/usr/share/ansible/collections
  executable location = /root/.local/bin/ansible
  python version = 3.8.10 (default, Jun  2 2021, 10:49:15) [GCC 9.4.0]
  jinja version = 3.0.1
  libyaml = True
root@vagrant:~/netololgy_ansible# 
```
***
#### Задание 1. 

```buildoutcfg
root@vagrant:~/netololgy_ansible# ansible-playbook -i ./inventory/test.yml site.yml 

PLAY [Print os facts] ******************************************************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************************************************
ok: [localhost]

TASK [Print OS] ************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **********************************************************************************************************************************************************
ok: [localhost] => {
    "msg": 12
}

PLAY RECAP *****************************************************************************************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
> Значение some_fact=12
***

#### Задание 2.

> В файле /root/netololgy_ansible/group_vars/all меняем значение some_fact на "all default fact"
***
#### Задание 3.
> Запустил контейнеры
```buildoutcfg
root@vagrant:~/netololgy_ansible/inventory# docker ps 
CONTAINER ID   IMAGE                 COMMAND        CREATED          STATUS          PORTS     NAMES
b00fc49f52b6   pycontribs/ubuntu     "sleep 7200"   4 seconds ago    Up 3 seconds              ubuntu
a80f4db46b00   pycontribs/centos:7   "sleep 7200"   19 seconds ago   Up 18 seconds             centos7
root@vagrant:~/netololgy_ansible/inventory# 
```
***
#### Задание 4.
```buildoutcfg
root@vagrant:~/netololgy_ansible# ansible-playbook -i ./inventory/prod.yml site.yml 

PLAY [Print os facts] ******************************************************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************************************************
[DEPRECATION WARNING]: Distribution Ubuntu 18.04 on host ubuntu should use /usr/bin/python3, but is using /usr/bin/python for backward compatibility with prior Ansible 
releases. A future Ansible release will default to using the discovered platform python for this host. See 
https://docs.ansible.com/ansible/2.11/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in version 2.12. Deprecation 
warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **********************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el"
}
ok: [ubuntu] => {
    "msg": "deb"
}

PLAY RECAP *****************************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

root@vagrant:~/netololgy_ansible# 

```
> some_fact:

```buildoutcfg
 ok: [centos7] => {
    "msg": "el"
}
ok: [ubuntu] => {
    "msg": "deb"
} 
```
***
#### Задание 5.
> Отредактировал файлы в /root/netololgy_ansible/group_vars
***
#### Задание 6.
```buildoutcfg
root@vagrant:~/netololgy_ansible# ansible-playbook -i ./inventory/prod.yml site.yml 

PLAY [Print os facts] ******************************************************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************************************************
[DEPRECATION WARNING]: Distribution Ubuntu 18.04 on host ubuntu should use /usr/bin/python3, but is using /usr/bin/python for backward compatibility with prior Ansible 
releases. A future Ansible release will default to using the discovered platform python for this host. See 
https://docs.ansible.com/ansible/2.11/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in version 2.12. Deprecation 
warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **********************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *****************************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

root@vagrant:~/netololgy_ansible# 
```
***
#### Задание 7.
```buildoutcfg
root@vagrant:~/netololgy_ansible# ansible-vault encrypt ./group_vars/deb/examp.yml 
New Vault password: 
Confirm New Vault password: 
Encryption successful
root@vagrant:~/netololgy_ansible# 
root@vagrant:~/netololgy_ansible# ansible-vault encrypt ./group_vars/el/examp.yml 
New Vault password: 
Confirm New Vault password: 
Encryption successful
root@vagrant:~/netololgy_ansible#
```
***
#### Задание 8.
```buildoutcfg
root@vagrant:~/netololgy_ansible# ansible-playbook -i ./inventory/prod.yml site.yml --ask-vault-pass
Vault password: 

PLAY [Print os facts] ******************************************************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************************************************
[DEPRECATION WARNING]: Distribution Ubuntu 18.04 on host ubuntu should use /usr/bin/python3, but is using /usr/bin/python for backward compatibility with prior Ansible 
releases. A future Ansible release will default to using the discovered platform python for this host. See 
https://docs.ansible.com/ansible/2.11/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in version 2.12. Deprecation 
warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **********************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *****************************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

root@vagrant:~/netololgy_ansible# 
```
***
#### Задание 9.
```buildoutcfg
root@vagrant:~/netololgy_ansible# ansible-doc -t connection -l
[WARNING]: Collection splunk.es does not support Ansible version 2.11.4
[WARNING]: Collection ibm.qradar does not support Ansible version 2.11.4
[WARNING]: Collection frr.frr does not support Ansible version 2.11.4
ansible.netcommon.httpapi      Use httpapi to run command on network appliances                                                                                        
ansible.netcommon.libssh       (Tech preview) Run tasks using libssh for ssh connection                                                                                
ansible.netcommon.napalm       Provides persistent connection using NAPALM                                                                                             
ansible.netcommon.netconf      Provides a persistent connection using the netconf protocol                                                                             
ansible.netcommon.network_cli  Use network_cli to run command on network appliances                                                                                    
ansible.netcommon.persistent   Use a persistent unix socket for connection                                                                                             
community.aws.aws_ssm          execute via AWS Systems Manager                                                                                                         
community.docker.docker        Run tasks in docker containers                                                                                                          
community.docker.docker_api    Run tasks in docker containers                                                                                                          
community.docker.nsenter       execute on host running controller container                                                                                            
community.general.chroot       Interact with local chroot                                                                                                              
community.general.funcd        Use funcd to connect to target                                                                                                          
community.general.iocage       Run tasks in iocage jails                                                                                                               
community.general.jail         Run tasks in jails                                                                                                                      
community.general.lxc          Run tasks in lxc containers via lxc python library                                                                                      
community.general.lxd          Run tasks in lxc containers via lxc CLI                                                                                                 
community.general.qubes        Interact with an existing QubesOS AppVM                                                                                                 
community.general.saltstack    Allow ansible to piggyback on salt minions                                                                                              
community.general.zone         Run tasks in a zone instance                                                                                                            
community.kubernetes.kubectl   Execute tasks in pods running on Kubernetes                                                                                             
community.libvirt.libvirt_lxc  Run tasks in lxc containers via libvirt                                                                                                 
community.libvirt.libvirt_qemu Run tasks on libvirt/qemu virtual machines                                                                                              
community.okd.oc               Execute tasks in pods running on OpenShift                                                                                              
community.vmware.vmware_tools  Execute tasks inside a VM via VMware Tools                                                                                              
containers.podman.buildah      Interact with an existing buildah container                                                                                             
containers.podman.podman       Interact with an existing podman container                                                                                              
kubernetes.core.kubectl        Execute tasks in pods running on Kubernetes                                                                                             
local                          execute on controller                                                                                                                   
paramiko_ssh                   Run tasks via python ssh (paramiko)                                                                                                     
psrp                           Run tasks over Microsoft PowerShell Remoting Protocol                                                                                   
ssh                            connect via ssh client binary                                                                                                           
winrm                          Run tasks over Microsoft's WinRM                                                                                                        
root@vagrant:~/netololgy_ansible# 
```
***
#### Задание 10.
> Добавил новую группу:
```buildoutcfg
root@vagrant:~/netololgy_ansible# cat ./inventory/prod.yml 
---
  el:
    hosts:
      centos7:
        ansible_connection: docker
  deb:
    hosts:
      ubuntu:
        ansible_connection: docker
  local:
    hosts:
      localhost:
        ansible_connection: local
root@vagrant:~/netololgy_ansible# 
```
> Добавил group_vars:
```buildoutcfg
root@vagrant:~/netololgy_ansible# ls -l ./group_vars/
total 16
drwxr-xr-x 2 root root 4096 Aug 31 17:11 all
drwxr-xr-x 2 root root 4096 Aug 31 17:53 deb
drwxr-xr-x 2 root root 4096 Aug 31 17:54 el
drwxr-xr-x 2 root root 4096 Aug 31 18:06 local
root@vagrant:~/netololgy_ansible# 
```

***

#### Задание 11.
```buildoutcfg
root@vagrant:~/netololgy_ansible# ansible-playbook -i ./inventory/prod.yml site.yml --ask-vault-pass
Vault password: 

PLAY [Print os facts] ******************************************************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************************************************
ok: [localhost]
[DEPRECATION WARNING]: Distribution Ubuntu 18.04 on host ubuntu should use /usr/bin/python3, but is using /usr/bin/python for backward compatibility with prior Ansible 
releases. A future Ansible release will default to using the discovered platform python for this host. See 
https://docs.ansible.com/ansible/2.11/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in version 2.12. Deprecation 
warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **********************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "local default fact"
}
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *****************************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

root@vagrant:~/netololgy_ansible# 
```
***
#### Задание 12.
> Отправляем все изменения в репозиторий











