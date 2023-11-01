# Session 1 - Introduction & Setting up of Ansible

Ansible is an open-source automation and configuration management tool that simplifies and automates tasks related to the provisioning, configuration, deployment, and management of computer systems, applications, and network infrastructure. It is widely used in DevOps and IT operations to streamline and orchestrate complex IT processes.

Key features and concepts of Ansible include:

1. **Agentless**: Ansible is agentless, which means it doesn't require any software agents or additional daemons to be installed on the managed systems. It communicates with remote systems over SSH by default, making it easy to manage a wide range of devices and operating systems.

2. **Declarative Language**: Ansible uses a declarative language to describe the desired state of a system. You specify what you want the system to look like, and Ansible takes care of making it so. This makes Ansible configurations human-readable and easy to understand.

3. **Playbooks**: Automation scripts in Ansible are written in YAML format and are called "playbooks." Playbooks define a series of tasks and their desired states, which Ansible will execute on the managed hosts. Playbooks can be used for tasks like software installation, configuration management, and application deployment.

4. **Inventory**: Ansible uses an "inventory" file or dynamic inventory scripts to define the list of hosts or systems to be managed. The inventory can be a static file or generated dynamically based on your infrastructure.

5. **Modules**: Ansible provides a vast library of "modules" for performing specific tasks on managed hosts. Modules cover a wide range of actions, from package installation and service management to cloud provisioning and more.

6. **Idempotence**: Ansible promotes idempotent operations, which means you can repeatedly apply the same configuration without causing unintended changes or harm. This property is essential for ensuring the stability and predictability of automation tasks.

7. **Roles**: Ansible allows you to organize your playbooks and tasks into reusable components called "roles." Roles help modularize your configurations and make them more maintainable.

8. **Ad-Hoc Commands**: In addition to playbooks, Ansible also supports ad-hoc commands, which are one-time, immediate tasks that can be executed directly from the command line.

9. **Integration**: Ansible can integrate with various cloud providers, networking devices, containers, and external tools, making it a versatile tool for managing different aspects of an IT environment.

10. **Community and Ecosystem**: Ansible has a large and active community, which has created numerous roles, modules, and playbooks that can be shared and reused. There is also a commercial version called Ansible Tower, which offers additional features for enterprise use.

Ansible is known for its simplicity, ease of use, and flexibility. It can be used for a wide range of automation tasks, from system provisioning and configuration management to application deployment and orchestration. It is a popular choice in DevOps and infrastructure as code (IaC) workflows for automating and managing IT infrastructure.

Setting up Ansible can vary slightly depending on the operating system and environment you're using, but I can provide you with a general guide that you can adapt for your specific setup. You can use this guide as a starting point for creating your markdown documentation.

## Setting Up Ansible

### Prerequisites

Before setting up Ansible, ensure you have the following prerequisites:

- A server or computer to act as the Ansible control node. This could be your local machine or a dedicated server.
- Remote servers or devices that you want to manage with Ansible.
- SSH access to the remote servers.
- A basic understanding of YAML (for writing Ansible playbooks).

### Installation

1. **Ansible Control Node**:

   - If you are using a Linux-based system, you can install Ansible with your system's package manager. For example, on Ubuntu:

     ```shell
     sudo apt-get update
     sudo apt-get install ansible
     ```

   - On CentOS or Red Hat upto rhel8 :

     ```shell
     sudo yum install epel-release
     sudo yum install ansible
     ```
  
  - On  Red Hat 9 :

     ```shell
     sudo yum install ansible-core
     ```

   - If you prefer to use Python's package manager, you can install Ansible via `pip`:

     ```shell
     pip install ansible
     ```

2. **Inventory File**:

   Create an Ansible inventory file that lists the hosts or devices you want to manage. This file can be named, for example, `inventory.ini`. It might look like this:

   ```ini
   [web_servers]
   server1 ansible_host=192.168.1.100
   server2 ansible_host=192.168.1.101

   [db_servers]
   server3 ansible_host=192.168.1.102
   ```

3. **SSH Key Setup**:

   Ensure you have SSH key access to the remote servers. You may need to create an SSH key pair and copy the public key to the remote servers for passwordless access.

   ```shell
   ssh-keygen
   ssh-copy-id user@remote-server
   ```

4. **Testing**:

   Test your Ansible installation by running a simple `ping` command:

   ```shell
   ansible all -m ping -i inventory.ini
   ```

   This should ping all the hosts listed in your inventory file.

### Writing Playbooks

Ansible playbooks are written in YAML format. They describe a series of tasks to be performed on remote servers. Here is an example playbook that installs the Apache web server:

```yaml
- name: Install Apache
  hosts: web_servers
  tasks:
    - name: Update package cache
      apt:
        update_cache: yes
      become: yes

    - name: Install Apache
      apt:
        name: apache2
        state: present
      become: yes
```

### Running Playbooks

To execute a playbook, use the `ansible-playbook` command. For the example playbook above, run:

```shell
ansible-playbook -i inventory.ini install_apache.yml
```

This will install Apache on the servers listed in the `web_servers` group.

### Additional Resources

- [Ansible Documentation](https://docs.ansible.com/ansible/latest/index.html): The official documentation for Ansible provides comprehensive information on setup, usage, and best practices.
- [Ansible Galaxy](https://galaxy.ansible.com/): A repository of community-contributed roles and playbooks.
- [Ansible for DevOps](https://www.ansiblefordevops.com/): A book by Jeff Geerling, available for free online, that covers many Ansible topics.

Remember to adapt this guide to your specific environment and needs. You can use this information as a foundation for creating your markdown documentation.