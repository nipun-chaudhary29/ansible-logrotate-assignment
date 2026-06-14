# Ansible Log Management Assignment

This repository configures log rotation for `custom-monitor.service` using Ansible and logrotate.

## Repository structure

```text
.
├── inventory.yml
├── logrotate.yml
├── README.md
├── .gitignore
├── vars/
│   └── custom_monitor.yml
└── templates/
    └── custom-monitor-logrotate.j2
```

## Why variables are separate from tasks

The main playbook `logrotate.yml` contains only the automation tasks.
The file `vars/custom_monitor.yml` contains the configurable values, such as:

- service name
- log directory
- logrotate config path
- retention count
- rotation frequency
- owner/group/mode


## Before running

Download the private key from the assignment and place it in this directory:

```bash
sre-logrotate-0.pem
```

Set correct key permissions:

```bash
chmod 400 sre-logrotate-0.pem
```

The `.gitignore` file prevents `*.pem` files from being committed.

## Test SSH manually

```bash
ssh -i sre-logrotate-0.pem ubuntu@3.98.94.59
```

Exit after successful login:

```bash
exit
```

## Test Ansible connectivity

```bash
ansible -i inventory.yml sre-log-rotate -m ping
```

Expected result:

```text
sre-log-rotate | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

## Syntax check

```bash
ansible-playbook -i inventory.yml logrotate.yml --syntax-check
```

## Run the playbook

```bash
ansible-playbook -i inventory.yml logrotate.yml
```

## Verify idempotency

Run the playbook a second time:

```bash
ansible-playbook -i inventory.yml logrotate.yml
```

The second run should show no unnecessary changes if the system is already configured correctly.

## Verify on the remote host

```bash
ssh -i sre-logrotate-0.pem ubuntu@3.98.94.59
```

Check the log directory:

```bash
sudo ls -ld /var/log/custom-monitor
```

Check the generated logrotate config:

```bash
sudo cat /etc/logrotate.d/custom-monitor
```

Validate logrotate config manually:

```bash
sudo logrotate -d /etc/logrotate.d/custom-monitor
```