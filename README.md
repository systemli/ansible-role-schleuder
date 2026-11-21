ansible-role-schleuder
=================

[![Build Status](https://github.com/systemli/ansible-role-schleuder/workflows/Integration/badge.svg?branch=main)](https://github.com/systemli/ansible-role-schleuder/actions?query=workflow%3AIntegration)
[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-schleuder-blue.svg)](https://galaxy.ansible.com/systemli/schleuder/)


Install and maintain a [schleuder](https://0xacab.org/schleuder/schleuder)
service and it's schleuder lists.

### What is installed during the ansible run ?

This role will setup [schleuder](https://0xacab.org/schleuder/schleuder) and 
[schleuder-cli](https://0xacab.org/schleuder/schleuder-cli), by default.

Additionally you can install the schleuder-gitlab-ticketing-plugin and hook
your schleuder lists to a gitlab instance. Therefore you have set 
`schleuder_install_gitlab_ticket_plugin: True` in your vars file. 

### What else is needed? 

You need to have to setup and install a 
[mail transport](https://schleuder.org/schleuder/docs/server-admins.html#hook-into-mail-transport-agent)
on the server where schleuder is running. 

Please consult the [schleuder documentation](https://schleuder.org/schleuder/docs/server-admins.html)
for additional information.

Download
--------

Download latest release with `ansible-galaxy`

	ansible-galaxy install systemli.schleuder

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: systemli.schleuder }

Testing & Development
---------------------

Tests
-----

For developing and testing the role we use GitHub Actions and Molecule with the Docker driver.
CI runs the role in a systemd-enabled container on Debian 13.

```
molecule test
```

Use `molecule converge` to only apply the role and `molecule login` to open a shell in the container.
This requires Docker (or a compatible container runtime), `molecule` and `molecule-plugins[docker]`.

License
-------

GPLv3

Author Information
------------------

https://www.systemli.org
