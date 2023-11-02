# Session 3 - Ansible configuration file & Ansible Facts

## ad hoc cmd for installing software other than root user : 
- Privilege Escalation Options:
  control how and which user you become as on target hosts

  **--become-method**
        privilege escalation method to use (default=sudo), use `ansible-doc -t become -l` to list valid choices.
  
  **--become-user**
        run operations as this user (default=root)

  **--become**
        run operations with become (does not imply password prompting)

    ```shell
    ansible all -m package -a "name=dialog state=installed" --become --become-method=sudo --become-user=root --ask-become-pass
    ```

cd ~/.ssh

 vim /etc/ansible/hosts 
```shell
    192.168.1.48     ansible_user=devops     ansible_password=redhat        ansible_become_password=redhat
```

```shell
vim /etc/ansible/ansible.cfg 
```

```shell

[privilege_escalation]

become=True
become_ask_pass=False
become_method=sudo
become_user=root

```

on managed node

```shell
useradd devops
```
```shell
passwd devops
```


vim /etc/sudoers

```shell
## Allow root to run any commands anywhere 

root    ALL=(ALL)       ALL

devops  ALL=(ALL)       ALL
```

## This command will gather facts from the host at IP address 192.168.1.48 and filter the results based on the ansible_user variable, displaying information related to the user configuration on the remote host.
```shell
ansible 192.168.1.48 -m setup -a "filter=ansible_user*"
```
- `-m setup`: Specifies the Ansible module to use, which is `setup` for gathering system facts.
- `-a "filter=ansible_user*"`: This option allows you to filter the facts based on the `ansible_user` variable. You can replace `ansible_user*` with any specific filter criteria you want.



## Creating groupname **[control_node]** in host group
```shell
vim /etc/ansible/hosts 
```

```/etc/ansible/hosts
[control_node]
192.168.1.48     ansible_user=devops     ansible_password=redhat        ansible_become_password=redhat
```