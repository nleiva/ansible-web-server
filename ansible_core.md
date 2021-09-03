# Running from Ansible Core

## Requirements

Ansible 2.9+ needs to be [installed](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-with-pip) in your computer to run this example. [Python3](https://wiki.python.org/moin/BeginnersGuide/Download) as well.

```bash
python -m pip install --user ansible
```

### Python libraries

As we interact with Azure, we need a couple of Python libraries to be present in the system.

```bash
pip install --user -r requirements.txt
```

### Ansible Collections

We also need the Ansible [collection for Azure](https://github.com/ansible-collections/azure#ansible-collection-for-azure).

```bash
ansible-galaxy collection install -r collections/requirements.yml
```

### SSH Public key

You need to provide your SSH Key pair, so Azure can add your public SSH Key to `~/.ssh/authorized_keys` in the instances it creates. Hence, Ansible can use the Private Key to configure these instances.

You can pass the location of these files via `filepath1` and `filepath2`. They default to `~/.ssh/id_rsa.pub` and `~/.ssh/id_rsa` respectively.

```yaml
username: "{{ lookup('env', 'USER') }}" # Pass as an extra-vars to override
pubkey_file: "{{ filepath1 | default('~/.ssh/id_rsa.pub') }}"
private_key_file: "{{ filepath2 | default('~/.ssh/id_rsa') }}"
ssh_pubkey: "{{ lookup('file', pubkey_file) }}"
ssh_private_key: "{{ lookup('file', private_key_file) }}"
```

## Creating the Web Server

Follow these steps to provision the Web Server.

1. Clone this repository: `git clone https://github.com/nleiva/ansible-webserver-azure.git`

2. Make your [Azure service principal parameters](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal) (`AZURE_SUBSCRIPTION_ID`, `AZURE_CLIENT_ID`, `AZURE_SECRET`, and `AZURE_TENANT`) available as environment variables (`export`).

```bash
export AZURE_SUBSCRIPTION_ID='...'
export AZURE_CLIENT_ID='...'
export AZURE_SECRET='...'
export AZURE_TENANT='...'
```

3. Define the number and operating system of the backend servers. The default [vms file](vars/vms.yml) defines 2 instances; one running `centos`, and the other one `ubuntu` (these are the two distributions supported at the moment).

```yaml
vms:
  1: centos
  2: ubuntu
```

4. Run the [Playbook](create_resources.yml) and wait a couple of minutes while the Load Balancer and VM(s) are being provisioned and the required software is installed:

```bash
⇨  ansible-playbook create_resources.yml -v

PLAY [Create Azure Resources] *********************************************************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************************************************
ok: [localhost]

TASK [check_vars : Check AZURE_SUBSCRIPTION_ID is defined as an environment variable] *************************************************************************************************
skipping: [localhost] => {
    "changed": false,
    "skip_reason": "Conditional result was False"
}

<snip>

PLAY RECAP ****************************************************************************************************************************************************************************
localhost                  : ok=38   changed=15   unreachable=0    failed=0    skipped=12   rescued=0    ignored=0   
testbed-vm11               : ok=14   changed=7    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
testbed-vm22               : ok=15   changed=9    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   
```

## Deleting the resources

Follow these steps to delete all resources.

1. Run the [delete_resources](delete_resources.yml).

```bash
ansible-playbook delete_resources.yml -v
```