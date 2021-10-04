# AWS

## Requirements

### Python libraries

As we will interact with AWS, we need a couple of Python libraries to be present in the system.

```bash
pip install --user -r requirements_aws.txt
```

### Ansible Collections

We will also need the Ansible [Amazon AWS Collection](https://github.com/ansible-collections/amazon.aws#amazon-aws-collection).

```bash
ansible-galaxy collection install -r collections/requirements.yml
```

## Launching the Web App

Follow these steps.

1. Clone this repository: `git clone https://github.com/nleiva/aws-testbed.git`

2. Make your [AWS account credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) (`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`) available as environment variables (`export`).

```bash
export AWS_ACCESS_KEY_ID='...'
export AWS_SECRET_ACCESS_KEY='...'
```

3. Run it.

```bash
ansible-navigator run main.yml -e "cloud=aws dns_zone=sandbox760.opentlc.com"
```