# ansible-learning

## Weekly Journals :

- [Session 1 - Introduction & Setting up of Ansible](session-1/readme.md)
- [Session 2 - Complete overview of Playbook](session-2/readme.md)
- [Session 3 - Ansible configuration file & Ansible Facts](session-3/readme.md)
- [Session 4 - Installing & Configuring of Docker with Ansible](session-4/readme.md)
- [Session 5 - Template & Jinja2](session-5/readme.md)
- [Session 6 - Setting up load balancer & Ansible Roles](session-6/readme.md)

## Extras
- [Github Markdown TOC Generator](https://ecotrust-canada.github.io/markdown-toc/)
# **Ansible Hands-on Guide for Beginners**

## **1. Introduction to Ansible**

Ansible is an open-source automation tool used for **configuration management, application deployment, and task automation**. It is agentless and uses **SSH** to communicate with remote machines.

---

## **2. Removing Unnecessary Software**

Before starting, remove any unnecessary software like `firefox`, `php`, and `httpd`.

### **Command:**

```bash
sudo yum remove -y firefox php httpd
```

### **Explanation:**

- `yum remove` → Removes the specified packages.
- `-y` → Automatically confirms removal without asking.
- `firefox php httpd` → List of packages to be removed.

### **Verify Removal:**

```bash
rpm -q firefox php httpd
```

- This command checks if the packages are still installed.
- If a package is not installed, it will return: `package <name> is not installed`.

---

## **3. Understanding Ansible Inventory Structure**

Ansible uses an **inventory file** (`/etc/ansible/hosts`) to manage hosts.

### **Edit the Inventory File:**

```bash
gedit /etc/ansible/hosts
```

### **Example Inventory File Content:**

```ini
[local]
localhost ansible_connection=local

[webservers]
192.168.1.10
192.168.1.11
```

### **Explanation:**

- `[local]` → Defines a local group.
- `localhost ansible_connection=local` → Configures Ansible to run locally.
- `[webservers]` → Defines a group for web servers.
- IPs `192.168.1.10` & `192.168.1.11` are the managed nodes.

To list all hosts in inventory:

```bash
ansible all --list-hosts
```

---

## **4. Running Ad-hoc Ansible Commands**

Ad-hoc commands are one-time Ansible commands that don't require a playbook.

### **Test Connectivity with Hosts:**

```bash
ansible all -m ping
```

#### **Expected Output:**

```json
localhost | SUCCESS => {
    "ping": "pong"
}
```

### **Run Shell Commands on Remote Hosts:**

```bash
ansible all -m command -a "whoami"
```

```bash
ansible all -m command -a "date"
```

### **Explanation:**

- `ansible all` → Runs the command on all hosts in the inventory.
- `-m command` → Uses the **command module** to execute shell commands.
- `-a "whoami"` → Executes the `whoami` command to check the logged-in user.

---

## **5. Installing and Managing Packages via Ansible**

### **Install `httpd` (Apache Web Server):**

```bash
ansible all -m package -a "name=httpd state=present"
```

### **Install `dialog` as Root (With Sudo Privileges):**

```bash
ansible all -m package -a "name=dialog state=present" \
  --become --become-method=sudo --become-user=root --ask-become-pass
```

### **Understanding Different States in Ansible Modules**

| **State**    | **Description** |
|-------------|----------------|
| `present`   | Ensures the package is installed. |
| `absent`    | Ensures the package is removed. |
| `latest`    | Ensures the package is installed with the latest version. |
| `installed` | Alias for `present`. |
| `removed`   | Alias for `absent`. |

### **Commonly Used Ansible Modules**

| **Module**     | **Purpose** |
|---------------|------------|
| `ping`        | Checks connectivity with hosts. |
| `command`     | Runs shell commands on remote hosts. |
| `package`     | Installs, removes, or manages packages. |
| `copy`        | Copies files from the control node to remote hosts. |
| `service`     | Manages services (start, stop, restart, enable). |
| `firewalld`   | Configures firewall rules. |

---

## **6. Ansible Playbooks**

Playbooks are **YAML files** that define a set of tasks to automate complex processes.

### **Example Playbook: Install and Start Apache**

```yaml
- hosts: webservers
  become: yes
  tasks:
    - name: Install Apache
      package:
        name: httpd
        state: present
    
    - name: Start Apache Service
      service:
        name: httpd
        state: started
        enabled: yes
```

### **Explanation:**

- `hosts: webservers` → Applies tasks to the `webservers` group.
- `become: yes` → Uses root privileges.
- `package:` → Installs Apache (`httpd`).
- `service:` → Starts and enables Apache.

### **Run the Playbook:**

```bash
ansible-playbook apache.yml
```

---

## **7. Summary of Useful Ansible Commands**

| **Command**                                                                          | **Description**                                   |
| ------------------------------------------------------------------------------------ | ------------------------------------------------- |
| `ansible all -m ping`                                                                | Check if hosts are reachable                      |
| `ansible all -m command -a "whoami"`                                                 | Check the user running Ansible on remote machines |
| `ansible all --list-hosts`                                                           | Show all managed hosts                            |
| `ansible all -m command -a "date"`                                                   | Get the current date on all hosts                 |
| `ansible all -m package -a "name=httpd state=present"`                             | Install `httpd` using Ansible                     |
| `ansible all -m package -a "name=dialog state=present" --become --ask-become-pass` | Install `dialog` with sudo privileges             |
| `ansible-playbook apache.yml`                                                        | Run an Ansible playbook                           |

---


