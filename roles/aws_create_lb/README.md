AWS Create LB
=============

A brief description of the role goes here.

Requirements
------------

The boto package is required.

```
pip3 install boto3
```

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

We need to export `AWS_ACCESS_KEY` and `AWS_SECRET_KEY` before executing.


Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: localhost
      roles:
         - { role: ansible-webserver-azure.azure_create_lb }

License
-------

BSD
