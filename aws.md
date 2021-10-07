# AWS

You need to install [ansible-navigator](https://github.com/ansible/ansible-navigator#installing).

```bash
pip3 install 'ansible-navigator[ansible-core]'
```

## Launching the Web App

Follow these steps.

1. Clone this repository: `git clone https://github.com/nleiva/aws-testbed.git`

2. Make your [AWS account credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) (`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`) available as environment variables (`export`).

```bash
export AWS_ACCESS_KEY_ID='...'
export AWS_SECRET_ACCESS_KEY='...'
```

3. Select AWS as the cloud option (`cloud`) and your domain name (`dns_zone`) if you want to create Route53 entries (optional). Run it.

```bash
ansible-navigator run main.yml -e cloud=aws -e dns_zone=sandbox760.opentlc.com
```

4. Navigate to http://testbed.dns_zone

## Deleting the App

```bash
ansible-navigator run main.yml -e cloud=aws -e delete=true
```

**Note**: I use [podman](https://podman.io/) as my container engine (`container-engine`). You can change to another alternative in the ansible [navigator config file](ansible-navigator.yml).

