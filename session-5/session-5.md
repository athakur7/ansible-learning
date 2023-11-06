# Session 5 - Template & Jinja2
Certainly! Here's a Markdown documentation that explains Ansible templates and the Jinja2 templating language with real-life examples:

# Ansible Template and Jinja2 Documentation

## Overview

This documentation provides an introduction to Ansible templates and the Jinja2 templating language. Ansible templates allow you to dynamically generate text files by combining templates with variables and facts. We'll cover the basics and provide real-life examples to help you get started.

## Prerequisites

- Basic knowledge of Ansible and YAML.
- Ansible installed on your local system.
- Understanding of variables and facts in Ansible.

## What are Ansible Templates?

Ansible templates are used to create dynamic text files that can be tailored for specific hosts or environments. These files often contain placeholders that are replaced with actual values using the Jinja2 templating language.

## Jinja2 Templating Language

Jinja2 is the templating engine used by Ansible to render templates. It allows you to insert variables, apply filters, use control structures, and more. Jinja2 placeholders are enclosed in double curly braces, e.g., `{{ variable_name }}`.

### Example 1: Basic Variable Insertion

Let's create a simple template that inserts a variable value into a text file. Suppose we have a variable `server_name`:

```yaml
# Template file (template.txt.j2)
Welcome to {{ server_name }} server!

# Playbook task
- name: Render template
  template:
    src: template.txt.j2
    dest: /etc/motd
```

In this example, `{{ server_name }}` will be replaced with the value of the `server_name` variable.

### Example 2: Conditional Statements

You can use conditional statements in templates to generate content based on conditions. For instance, you can enable or disable a feature in a configuration file:

```yaml
# Template file (nginx.conf.j2)
{% if enable_feature %}
feature_enabled on;
{% else %}
feature_enabled off;
{% endif %}

# Playbook task
- name: Render template with condition
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
```

Here, the content of the template depends on the value of the `enable_feature` variable.

### Example 3: Looping

You can loop through lists in your templates to generate repetitive content. For example, configuring multiple users in an SSH configuration file:

```yaml
# Template file (sshd_config.j2)
{% for user in ssh_users %}
AllowUsers {{ user }}
{% endfor %}

# Playbook task
- name: Render template with loop
  template:
    src: sshd_config.j2
    dest: /etc/ssh/sshd_config
```

This template generates a list of users in the SSH configuration file.

## Benefits of Using Templates

- Dynamic configuration files.
- Reusability across multiple hosts.
- Maintainability and consistency.
- Flexible manipulation of variables and facts.

## Additional Resources

- [Ansible Documentation on Templates](https://docs.ansible.com/ansible/latest/user_guide/templates.html)
- [Jinja2 Documentation](https://jinja.palletsprojects.com/en/3.0.x/templates/)

This documentation provides an introduction to Ansible templates and Jinja2, along with real-life examples. Experiment with templates and the Jinja2 language to create dynamic and adaptable configurations in your Ansible playbooks.

## Check SeLinux Status
```shell
sudo getenforce
```

## Disable SeLinux
```shell
sudo setenforce 0
```

## The `sudo netstat -tnlp` command is used to display a list of active network connections and associated listening processes on a Linux system. Here's what each part of the command does:

- `netstat`: This is the command-line tool for displaying network-related information, including network connections and statistics.

- `-tnlp`: These are various options and filters for the `netstat` command:
  - `-t`: Display TCP connections.
  - `-n`: Show numerical addresses instead of resolving hostnames.
  - `-l`: Show only listening sockets (those waiting for incoming connections).
  - `-p`: Show the process ID and name of the program associated with each socket.

When you run `sudo netstat -tnlp`, you'll see a list of all active listening sockets on your system, along with the associated processes, their process IDs, and the port numbers they are listening on. This information is valuable for monitoring network activity and troubleshooting network-related issues.

## Ansible Handlers
- Sometimes you want a task to run only when a change is made on a machine. For example, you may want to restart a service if a task updates the configuration of that service, but not if the configuration is unchanged. Ansible uses handlers to address this use case. Handlers are tasks that only run when notified.
- [Ansible Handlers](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_handlers.html)

### Ansible Handler Example :
```yml
- hosts: aws_control_node,local_control_node
  # Specifying variable file 
  vars_files:
  - vars.yml
  tasks:
    - name: install web package
      package:
        name: " {{ web_package }} " 
        state: present
    - debug:
            var : webpage

    - name: conf changes in my apache web web-server
      template:
        dest: "/etc/httpd/conf.d/anand.conf"
        src: "web.conf.j2"
      notify: "restart web-server"

    - name: Deploy web package
      template:
        src: "{{ webpage }}"
        dest: "{{ web_doc_root }}/index.html"
    
    - name: Start web-server
      service:
        name: "{{ web_package }}"
        state: started
  # https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_handlers.html
  handlers:
    - name: restart web-server
      service:
        name: "{{ web_package }}"
        state: restarted
```
Certainly! Here's a Markdown file that explains how to skip tasks in Ansible playbooks using the `when` statement:


# Skipping Tasks in Ansible Playbooks
## https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_conditionals.html

In Ansible, you can control the execution of tasks within your playbook by using the `when` statement. This allows you to skip tasks based on specific conditions. This guide will show you how to skip tasks using the `when` statement in Ansible playbooks.

## Basic Syntax

The `when` statement is used to conditionally execute a task. When the specified condition is met, the task will run; otherwise, it will be skipped. The basic syntax for the `when` statement is as follows:

```yaml
- name: Name of the task
  module_name: Module-specific options
  when: Condition
```

- `name`: A descriptive name for the task.
- `module_name`: The module that performs the task (e.g., `command`, `shell`, `apt`, `yum`, etc.).
- `Condition`: The condition that determines whether the task should run or be skipped.

## Examples

### Skip Task If a Variable Is Not Defined

You can skip a task if a variable is not defined. Here's an example:

```yaml
- name: Task will be skipped if my_variable is not defined
  command: some_command
  when: my_variable is defined
```

### Skip Task If a Variable Has a Specific Value

You can skip a task if a variable has a specific value. For example:

```yaml
- name: Task will be skipped if my_variable is not "some_value"
  command: some_command
  when: my_variable != "some_value"
```

### Skip Task If a File or Directory Exists

You can skip a task if a file or directory exists on the target host. Here's an example:

```yaml
- name: Task will be skipped if a file or directory exists
  command: some_command
  when: not (ansible_facts['file_exists_var'].stat.exists)
```

### Skip Task If a Previous Task Failed

You can skip a task if a previous task has failed by checking whether the `ansible_failed_task` variable is defined:

```yaml
- name: Task will be skipped if a previous task failed
  command: some_command
  when: ansible_failed_task is not defined
```

### Skip Task Based on Multiple Conditions

You can skip a task based on a combination of conditions using logical operators. For example:

```yaml
- name: Task will be skipped based on multiple conditions
  command: some_command
  when: (condition1 or condition2) and not condition3
```

By using the `when` statement and specifying the appropriate condition, you have fine-grained control over which tasks are executed and which are skipped in your Ansible playbooks.

These examples demonstrate how to skip tasks based on various conditions using the `when` statement in Ansible. You can customize these conditions to suit your specific requirements and orchestrate your playbooks more effectively.

Certainly! Here's a Markdown file that explains the usage of `echo $?` to display the exit status of the last executed command in a Unix-like shell:

# Understanding Exit Status with `echo $?`

In a Unix-like shell, the exit status of a command provides information about whether the command executed successfully or encountered an error. The exit status is a numeric value that can be accessed using the `echo $?` command immediately after executing another command. Here's how it works:

## Exit Status Values

- **0**: A command with an exit status of 0 indicates that the command executed successfully without any errors. This is the desired outcome for most commands.

- **Non-zero Value (usually 1 or higher)**: An exit status with a non-zero value usually indicates that the command encountered an error or failed to execute as expected. The specific non-zero value may vary depending on the command and the nature of the error.

## Usage of `echo $?`

To access the exit status of the last executed command, you can use the following command immediately after the command of interest:

```bash
echo $?
```

- `echo`: This is the command used to display the exit status value.
- `$?`: This is a special variable that holds the exit status of the previous command. It is automatically set by the shell.

## Example

Let's illustrate the usage of `echo $?` with a simple example. Suppose you want to check if a file exists using the `ls` command:

```bash
ls some_file.txt
echo $?
```

- If `some_file.txt` exists, the `ls` command will have an exit status of 0, and `echo $?` will display `0`.

- If `some_file.txt` does not exist, the `ls` command will have a non-zero exit status (typically 1), and `echo $?` will display the non-zero value.

## Use Cases

Understanding the exit status of commands is valuable for scripting and automation. You can use it to:

- Conditionally execute other commands based on the success or failure of a previous command.
- Handle errors or exceptions in scripts.
- Perform error checking and reporting in automated processes.

By using `echo $?`, you can gain insight into the outcome of command execution and take appropriate actions based on the exit status.

# [Error handling in playbooks](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_error_handling.html)

When Ansible receives a non-zero return code from a command or a failure from a module, by default it stops executing on that host and continues on other hosts. However, in some circumstances, you may want different behavior. Sometimes a non-zero return code indicates success. Sometimes you want a failure on one host to stop execution on all hosts. Ansible provides tools and settings to handle these situations and help you get the behavior, output, and reporting you want.

## Ignoring failed commands
By default, Ansible stops executing tasks on a host when a task fails on that host. You can use ignore_errors to continue despite of the failure.

```yaml
- name: Do not count this as a failure
  ansible.builtin.command: /bin/false
  ignore_errors: true
```
The `ignore_errors` directive only works when the task can run and returns a value of ‘failed’. It does not make Ansible ignore undefined variable errors, connection failures, execution issues (for example, missing packages), or syntax errors.

# [Ansible jinja filters ](https://linux-system-roles.github.io/documentation/howto/working-with-ansible-jinja-code-and-filters)

# `ANSIBLE_STDOUT_CALLBACK`
- `ANSIBLE_STDOUT_CALLBACK` environment variable is used to specify the callback plugin that should be used to format and display the output of Ansible playbooks or ad-hoc commands. Callback plugins control how Ansible displays information, such as task execution results, on the command line or in log files.

In your command:

```bash
ANSIBLE_STDOUT_CALLBACK=debug ansible-playbook -vv webserver-v3.yml
```

- `ANSIBLE_STDOUT_CALLBACK=debug` sets the `ANSIBLE_STDOUT_CALLBACK` environment variable to "debug." This means that the "debug" callback plugin will be used to format and display the output.

- `ansible-playbook` is the Ansible command for running playbooks.

- `-vv` is used to enable verbose output, providing more detailed information about playbook execution. It increases the verbosity level, making Ansible print more debug information.

- `webserver-v3.yml` is the name of the Ansible playbook (in this case, "webserver-v3.yml") that you want to execute.

By setting `ANSIBLE_STDOUT_CALLBACK` to "debug," you instruct Ansible to use the "debug" callback plugin to format and display the playbook's output. The "debug" callback plugin is useful for debugging and provides detailed information about task execution, variables, and other aspects of the playbook run.

This can be helpful when you want to inspect the internal details of how Ansible is processing and executing tasks within the playbook. The `-vv` flag further enhances the verbosity of the output, making it useful for troubleshooting and understanding the playbook's behavior.