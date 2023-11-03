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