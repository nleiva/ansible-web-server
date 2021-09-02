Azure Delete AKS
================

A brief description of the role goes here.

Requirements
------------

Several Python [packages are required](https://github.com/ansible-collections/azure/blob/dev/requirements-azure.txts).

```
pip3 install -r requirements_aks.txt
```


Role Variables
--------------

To (authenticate via service principal)[(https://docs.ansible.com/ansible/latest/collections/azure/azcollection/azure_rm_aks_module.html#notes)], provide these variables; `subscription_id`, `client_id`, `secret` and `tenant` or set them as environment variables;`AZURE_SUBSCRIPTION_ID`, `AZURE_CLIENT_ID`, `AZURE_SECRET` and `AZURE_TENANT`.

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: localhost
      roles:
         - { role: ansible-kubernetes.azure_delete_aks }

License
-------

BSD
