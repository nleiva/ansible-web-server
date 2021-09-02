# Web Server in Azure with Ansible

## Requirements

Ansible 2.9+ needs to be [installed](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-with-pip) in your computer to run this example. [Python3](https://wiki.python.org/moin/BeginnersGuide/Download) as well.

```bash
python -m pip install --user ansible
```

### Python libraries

As we interact with AWS, we need a couple of Python libraries to be present in the system.

```bash
pip install --user -r requirements.txt
```

### Ansible Collections

We also need the Ansible [collection for Azure](https://github.com/ansible-collections/azure#ansible-collection-for-azure).

```bash
ansible-galaxy collection install -r collections/requirements.yml
```

### Azure credentials

To authenticate via service principal, provide these variables; `subscription_id`, `client_id`, `secret` and `tenant` or set them as environment variables;`AZURE_SUBSCRIPTION_ID`, `AZURE_CLIENT_ID`, `AZURE_SECRET` and `AZURE_TENANT`.

- `AZURE_SUBSCRIPTION_ID`: [Find your Azure subscription](https://docs.microsoft.com/en-us/azure/media-services/latest/setup-azure-subscription-how-to?tabs=portal)
- `AZURE_CLIENT_ID` and `AZURE_TENANT`: [Register an application with Azure AD and create a service principal](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#register-an-application-with-azure-ad-and-create-a-service-principal)
- `AZURE_SECRET`: [Create a new application secret](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#option-2-create-a-new-application-secret)

### SSH Public key

You need to provide your SSH Key pair, so Azure can add your public SSH Key to `~/.ssh/authorized_keys` of the instances it creates. Hence, Ansible can use the Private Key to configure these instances.

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

1. Clone this repository: `git clone https://github.com/nleiva/aws-testbed.git`

2. Make your [AWS account credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) (`AWS_ACCESS_KEY` and `AWS_SECRET_KEY`) available as environment variables (`export`).

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

4. Run the [Playbook](create_resources.yml) and wait a couple of minutes while the Load Balancer and VM(s) are being provisioned and the requiered software is installed:

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

## Accessing the Web Server

We distribute the traffic among the instances to prevent failure in case any of them fails. By default the web server is at http://testbed.eastus.cloudapp.azure.com/. you can modify this with the variable `prefix`, that by default is `testbed`.

### VM1

<p align="center">
<img height="400" src="./pictures/centos.png">
</p>

### VM2

<p align="center">
<img height="400" src="./pictures/ubuntu.png">
</p>

## Deleting the resources

Follow these steps to delete all resources.

1. Run the [delete_resources](delete_resources.yml).

```bash
ansible-playbook delete_resources.yml -v
```

## Run from an Execution Environment

You need [ansible-navigator installed](https://github.com/ansible/ansible-navigator#installing).

```bash
ansible-navigator run create_resources.yml
```

**Note**: I use [podman](https://podman.io/) as my container engine (`container-engine`). You can change to another alternative in the ansible [navigator config file](ansible-navigator.yml).