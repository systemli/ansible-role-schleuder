ansible-role-schleuder
=================

[![Build Status](https://github.com/systemli/ansible-role-schleuder/workflows/Integration/badge.svg?branch=main)](https://github.com/systemli/ansible-role-schleuder/actions?query=workflow%3AIntegration)
[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-schleuder-blue.svg)](https://galaxy.ansible.com/systemli/schleuder/)


Install and maintain a [schleuder](https://0xacab.org/schleuder/schleuder)
service and it's schleuder lists.

### What is installed during the ansible run ?

This role will setup [schleuder](https://0xacab.org/schleuder/schleuder),
[schleuder-cli](https://0xacab.org/schleuder/schleuder-cli), 
[schleuder-web](https://0xacab.org/schleuder/schleuder-web) by default.

The web interface is accessible via localhost:3000 and you can log in with 
schleuders default credentials ("root@localhost" - "slingit!"). 

Additionally you can install the schleuder-gitlab-ticketing-plugin and hook
your schleuder lists to a gitlab instance. Therefore you have set 
`schleuder_install_gitlab_ticket_plugin: True` in your vars file. 

### What else is needed? 

You need to have to setup and install a 
[mail transport](https://schleuder.org/schleuder/docs/server-admins.html#hook-into-mail-transport-agent)
on the server where schleuder is running. 
Additionally you need to have a running web server with a vhost pointing to your schleuder-web
interface and it's port. 

Please consult the [schleuder documentation](https://schleuder.org/schleuder/docs/server-admins.html)
for additional information.

### How can new lists/list admins manage their lists?

If a public key of the list admin is present, this role/schleuder will inform
you about the list creation.

Schleuder list admins can request their password by logging in through web
interface, after their list was created.

Role Variables
--------------

    ### Define which playbooks should be run:
    schleuder_install_web: True
    schleuder_install_cli: True
    schleuder_install_gitlab_ticket_plugin: False
    
    ### schleuder vars:
    schleuder_schleuder_user: "schleuder"
    schleuder_gpg_use_tor: True
    schleuder_gpg_tor_keyserver: "hkp://zkaan2xfbuxia2wpf7ofnkbz6r5zdbbvxbunvp5g2iebopbfc4iqmbad.onion"
    schleuder_admin_keys_path: "/var/lib/schleuder/adminkeys"
    
    schleuder_lists: []
      #- name: foobar@cryptolists.systemli.org
      #  admin: admin@systemli.org
      #  # be sure to copy public_key of list admin to
      #  # files/schleuder/adminkeys/{{ name }}_{{ admin }}.pub
      #  # else set admin_pubkey_present to false
      #  # ---
      #  send_list_key: True
      #  present: True
      #  # ---
      #  admin_pubkey_present: True
      #  # if admin_pubkey_present is set to false
      #  schleuder list will be created, but is not functional
    
    ### schleuder-web vars:
    schleuder_schleuder_web_repo: https://0xacab.org/schleuder/schleuder-web
    schleuder_schleuder_web_home: "/var/www/schleuder-web"
    schleuder_schleuder_web_user: schleuder-web
    schleuder_schleuder_web_path: "{{ schleuder_schleuder_web_home }}/schleuder-web"
    schleuder_schleuder_web_git_update: False
    schleuder_schleuder_web_systemd_path: "/etc/systemd/system/schleuder-web.service"
    schleuder_schleuder_web_environment_vars_path: "/etc/default/schleuder-web"
    # set to false will make rails server listen on localhost only
    schleuder_schleuder_web_allow_access_from_outside: True
    
    ### schleuder-gitlab-ticketing-plugin vars:
    schleuder_gitlab_plugin_repo: https://0xacab.org/schleuder/schleuder-gitlab-ticketing
    schleuder_gitlab_plugin_path: "/opt/local/gitlab-ticketing"
    schleuder_gitlab_plugin_git_update: False
    
    ### schleuder-cli vars:
    schleuder_cli_path: "/root/.schleuder-cli"
    
    ###### File Section
    
    ### schleuder/schleuder.yml.j2
    schleuder_lists_dir: "/var/lib/schleuder/lists"
    schleuder_listlogs_dir: "/var/lib/schleuder/lists"
    schleuder_plugins_dir: "/etc/schleuder/plugins"
    schleuder_filters_dir: "/usr/local/lib/schleuder/filters"
    schleuder_log_level: "warn"
    schleuder_keyserver: "hkps://keys.openpgp.org"
    schleuder_superadmin: "root@localhost"
    schleuder_smtp_settings:
      address: localhost
      port: 25
      #domain:
      #enable_starttls_auto:
      #openssl_verify_mode:
      #authentication:
      #user_name:
      #password:
    schleuder_database:
      production:
        adapter: "'sqlite3'"
        database: "/var/lib/schleuder/db.sqlite"
        pool: 5
        timeout: 5000
    schleuder_api:
      host: localhost
      port: 4443
      # Certificate and key to use. You can create new ones with `schleuder cert generate`.
      tls_cert_file: "/etc/schleuder/schleuder-certificate.pem"
      tls_key_file: "/etc/schleuder/schleuder-private-key.pem"
    
    # List of api_keys to allow access to the API.
    # Example:
    # valid_api_keys:
    #   - abcdef...
    #   - zyxwvu...
    schleuder_valid_api_keys: []
    
    
    ### schleuder/list-defaults.yml.j2
    schleuder_send_encrypted_only: "true"
    schleuder_receive_encrypted_only: "true"
    schleuder_receive_signed_only: "false"
    schleuder_receive_authenticated_only: "false"
    schleuder_receive_from_subscribed_emailaddresses_only: "true"
    schleuder_receive_admin_only: "false"
    schleuder_headers_to_meta:
      - from
      - to
      - cc
      - date
      - sig
      - enc
    schleuder_keep_msgid: "true"
    schleuder_keywords_admin_only:
      - subscribe
      - unsubscribe
      - delete-key
    schleuder_keywords_admin_notify:
      - add-key
    schleuder_internal_footer: ""
    schleuder_public_footer: ""
    schleuder_subject_prefix: ""
    schleuder_subject_prefix_in: ""
    schleuder_subject_prefix_out: ""
    schleuder_bounces_drop_all: "false"
    schleuder_bounces_drop_on_headers:
      x-spam-flag: "yes"
    schleuder_bounces_notify_admins: "true"
    schleuder_include_list_headers: "true"
    schleuder_include_openpgp_header: "true"
    schleuder_openpgp_header_preference: signencrypt
    schleuder_max_message_size_kb: 10240
    schleuder_lists_log_level: warn
    schleuder_logfiles_to_keep: 2
    # Available: en, de.
    schleuder_language: en
    schleuder_forward_all_incoming_to_admins: "false"
    
    ### schleuder-web/database.yml
    # schleuder-web uses it's own database to store user credentials
    schleuder_web_database:
      production:
        adapter: "'sqlite3'"
        database: "db/production.sqlite"
        pool: 5
        timeout: 5000
    
    ### schleuder/gitlab.yml
    # global settings
    schleuder_gitlab_default_subject_filters: []
    schleuder_gitlab_default_sender_filters: []
    schleuder_gitlab_default_gitlab_connection: {}
    
    # settings per list
    # see https://0xacab.org/schleuder/schleuder-gitlab-ticketing
    schleuder_gitlab_lists: []
    #  - test@schleuder.example.com:
    #      project: tickets
    #      namespace: support
    #      subject_filters:
    #        - 'ignore me'
    #  - testz@schleuder.example.com:
    #      gitlab:
    #        endpoint: https://gitlab2.example.com/api/v4
    #        token: aaaa
    #      sender_filters:
    #        - 'noreply@example\.com'

Download
--------

Download latest release with `ansible-galaxy`

	ansible-galaxy install systemli.schleuder

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: systemli.schleuder }

Extended Variables Example
--------------------------


    schleuder_lists:
      - name: foobar@cryptolists.systemli.org
        admin: admin@systemli.org
        # public key has to be in 
        # files/adminkeys/{{ name }}_{{ admin }}.pub
      - name: no_pub_key_of_listadmin@cryptolists.systemli.org
        admin: test@systemli.org
        admin_pubkey_present: False
        send_list_key: False
        # public key is not put to server
        # list is created but not functional
        # list_key is not not sent to subscribers
      - name: will_be_deleted@cryptolists.systemli.org
        admin: test@systemli.org
        state: absent
        # list will be deleted in the next ansible run         

   
    # gitlab settings per list
    # see https://0xacab.org/schleuder/schleuder-gitlab-ticketing
    schleuder_gitlab_lists:
      - foobar@cryptolists.systemli.org 
          project: tickets
          namespace: support
          subject_filters:
            - 'ignore me'
      - testz@schleuder.example.com:
          gitlab:
            endpoint: https://gitlab2.example.com/api/v4
            token: 1234aaaa1234
          sender_filters:
            - 'noreply@example\.com'


Testing & Development
---------------------

Tests
-----

For developing and testing the role we use Github Actions, Molecule, and Vagrant. On the local environment you can easily test the role with

Run local tests with:

```
molecule test 
```

Requires Molecule, Vagrant and `python-vagrant` to be installed.For developing and testing the role we use Travis CI, Molecule and Vagrant. On the local environment you can easily test the role with


This will spin up a virtual machine with Debian Buster. Afterwards you can connect to the Schleuder
web interface via: 

```
http://localhost:3000
```


License
-------

GPLv3

Author Information
------------------

https://www.systemli.org
