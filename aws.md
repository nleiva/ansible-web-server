# AWS

## Python libraries

As we will interact with AWS, we need a couple of Python libraries to be present in the system.

```bash
pip install --user -r requirements_aws.txt
```

## Ansible Collections

We will also need the Ansible [Amazon AWS Collection](https://github.com/ansible-collections/amazon.aws#amazon-aws-collection).

```bash
ansible-galaxy collection install -r collections/requirements.yml
```