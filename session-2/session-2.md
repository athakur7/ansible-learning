# Session 2 - Complete overview of Playbook

## To create a Python virtual environment in VS Code, you can follow these steps:

1. **Open Your Python Project Folder**:
   Open VS Code and navigate to the folder where you want to create your Python virtual environment. You can do this by selecting "File" > "Open Folder" from the menu and choosing the folder you want to work in.

2. **Open the Integrated Terminal**:
   To create a virtual environment, you'll need to use the integrated terminal in VS Code. You can open it by selecting "View" > "Terminal" from the menu.

3. **Create a Python Virtual Environment**:
   In the terminal, use the `python -m venv` command to create a virtual environment. For example, to create a virtual environment named "myenv," you can use:

   ```bash
   python -m venv myenv
   ```

   This command creates a new folder called "myenv" in your project directory, and it sets up a Python virtual environment inside that folder.

4. **Activate the Virtual Environment**:
   After creating the virtual environment, you need to activate it. The activation command varies depending on your operating system:

   - **On Windows**:
     ```bash
     .\myenv\Scripts\activate
     ```

   - **On macOS and Linux**:
     ```bash
     source myenv/bin/activate
     ```

   You should see the virtual environment's name in the terminal prompt, indicating that the virtual environment is active.

5. **Install Dependencies**:
   With the virtual environment activated, you can install Python packages and dependencies specific to your project. For example, you can use `pip` to install packages:

   ```bash
   pip install package-name
   ```

6. **Work in Your Virtual Environment**:
   While your virtual environment is active, any Python packages you install will be isolated to that environment. You can run Python scripts and work on your project without affecting the global Python environment.

7. **Deactivate the Virtual Environment**:
   To deactivate the virtual environment and return to your global Python environment, simply type:

   ```bash
   deactivate
   ```

8. **Set Up Virtual Environment in VS Code**:
   You can also configure VS Code to use this virtual environment for your project. Create a `.env` file in your project's root directory, and add the following line to it (replace "myenv" with the name of your virtual environment):

   ```
   PYTHONPATH=myenv
   ```

   Save the file, and VS Code will automatically use this virtual environment when you open your project folder.

By following these steps, you can create and work within a Python virtual environment in Visual Studio Code, allowing you to manage project-specific dependencies and isolate your project from the global Python environment.
## Installing collections with ansible-galaxy cmd below:

- For ansible firewall necessray packages
```shell
ansible-galaxy collection install ansible.posix
```

- For checking installation present
```shell 
ansible-galaxy collection list
```
## Ansible adhoc commands:

### To check all availabe hosts/inventory

```ansible
ansible all --list-hosts
````

### To install softwares

```ansible
ansible all -m  package -a "name=httpd state=installed"
```
```ansible
ansible all -m  package -a "name=httpd state=present"
```

### To copy file
```ansible
ansible all -m copy -a "src=/web.html dest=/var/www/html"
```

### To start service
```ansible
ansible all -m service -a "name=httpd state=started enabled=yes"
```

### To enable firewall on a specific port

```ansible
ansible all -m firewalld -a "immediate=yes port=80/tcp state=enabled permanent=yes"
```

### reboot all the hosts

```ansible
ansible all -m reboot
```

### to run cmd like we do in terminal in different hosts

```ansible
ansible all -m command -a date
```

### ansible doc

```shell
ansible-doc -l
```

- **Search for specific package in ansible-doc:**
```shell
ansible-doc -l | grep package
```

## ansible-playbook

- To get some output of the playbook/ verbosity add **-v** in the playbook cmd
```ansible
ansible-playbook httpd-server-v2.yml -v
```