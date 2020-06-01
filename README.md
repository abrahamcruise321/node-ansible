# Node Ansible

Ansible scripts to setup Matic validator node

### Setup

**Copy `pem` private key file as `.workspace/private.pem`** to enable ssh through ansible.

### Inventory

Add hosts into `inventory.yml` under appropriate heads.

Available inventory group names:

* `sentry`
* `validator`

Add sentry nodes IPs/hosts under the `sentry` group and add validator node's IP/host under `validator` group. 

Example:

```yml
all:
  hosts:
  children:
    sentry:
      hosts:
        xxx.xxx.xx.xx: # <----- Add IP for sentry node
        xxx.xxx.xx.xx: # <----- Add IP for sentry node
    validator:
      hosts:
        xxx.xxx.xx.xx: # <----- Add IP for validator node
```

To check if nodes are reachable, run following commands:

```bash
# to check if sentry nodes are reachable
ansible sentry -m ping

# to check if validator nodes are reachable
ansible validator -m ping
```

### Sentry node setup

To show list of hosts where the playbook will run (notice `--list-hosts` at the end):

```bash
ansible-playbook -l sentry playbooks/mainnet.yml --extra-var="bor_branch=v0.2.0 heimdall_branch=v0.2.0 mainnet_version=mainnet-v1 node_type=sentry/sentry" --list-hosts
```

To run actual playbook on sentry nodes:

```bash
ansible-playbook -l sentry playbooks/mainnet.yml --extra-var="bor_branch=v0.2.0 heimdall_branch=v0.2.0 mainnet_version=mainnet-v1 node_type=sentry/sentry"
```

### Validator node setup (with sentry)

To show list of hosts where the playbook will run (notice `--list-hosts` at the end):

```bash
ansible-playbook -l validator playbooks/mainnet.yml --extra-var="bor_branch=v0.2.0 heimdall_branch=v0.2.0 mainnet_version=mainnet-v1 node_type=sentry/validator" --list-hosts
```

To run actual playbook on validator node:

```bash
ansible-playbook -l validator playbooks/mainnet.yml --extra-var="bor_branch=v0.2.0 heimdall_branch=v0.2.0 mainnet_version=mainnet-v1 node_type=sentry/validator"
```

### Validator node setup (with-out sentry)

To show list of hosts where the playbook will run (notice `--list-hosts` at the end):

```bash
ansible-playbook -l validator playbooks/mainnet.yml --extra-var="bor_branch=v0.2.0 heimdall_branch=v0.2.0 mainnet_version=mainnet-v1 node_type=without-sentry" --list-hosts
```

To run actual playbook on validator node:

```bash
ansible-playbook -l validator playbooks/mainnet.yml --extra-var="bor_branch=v0.2.0 heimdall_branch=v0.2.0 mainnet_version=mainnet-v1 node_type=without-sentry"
```

### Management commands

**To clean deployed setup (warning: this will delete all blockchain data)**

```bash
ansible-playbook -l <group-name> playbooks/clean.yml
```

**To show Heimdall account**

```bash
ansible-playbook -l <group-name> playbooks/show-heimdall-account.yml
```

**To increase ulimit**

```bash
ansible-playbook -l <group-name> playbooks/ulimit.yml
```

**To setup prometheus exporters**

```bash
ansible-playbook -l <group-name> playbooks/setup-exporters.yml
```

**To reboot machine**

```bash
ansible-playbook -l <group-name> playbooks/reboot.yml
```

### Stand-alone build

**To setup Heimdall**

```bash
ansible-playbook -l <group-name> --extra-var="heimdall_branch=v0.2.0" playbooks/heimdall.yml
```

To show list of hosts where the playbook will run:

```bash
ansible-playbook -l <group-name> --extra-var="heimdall_branch=v0.2.0" playbooks/heimdall.yml --list-hosts
```

**To setup Bor**

```bash
ansible-playbook -l <group-name> --extra-var="bor_branch=v0.2.0" playbooks/bor.yml
```

To show list of hosts where the playbook will run:

```bash
ansible-playbook -l <group-name> --extra-var="bor_branch=v0.2.0" playbooks/bor.yml --list-hosts
```

### Adhoc queries

**Ping**

Just to see if machines are reachable:

For sentry nodes:

```bash
ansible sentry -m ping
```

For validator nodes:

```
ansible validator -m ping
```

`ping` is a module name. You can any module and arguments here.

**Run shell command**

Following command will fetch and print all disk space stats from all `sentry` hosts.

For sentry nodes:

```bash
ansible sentry -m shell -a "df -h"
```

For validator nodes:

```bash
ansible validator -m shell -a "df -h"
```