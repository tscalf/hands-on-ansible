# Working with the Ansible Inventory.

## Create the Inventory for our Vagrant infrastructure.
See exercise/inventory. Notice groups of groups need [_groupname_:children] and
group vairables need [_grounpname_:vars]

## Migrate into a directory structure for Ansible.
See the Production and Test diretories. `group_vars` and `host_vars` files are in YML format.

### Order of precedence for Variables
1. group_vars/all
2. group_vars/_groupname_
3. host_vars/_hostname_

## Creating a context sensitive user.
ansible webservers -i inventory_prod -m user -a "name={{username}} password=123456" -sudo

|              | group_vars/all | group_vars/db | group_vars/webservers | host_vars/web1 |
| ------------ | -------------- | ------------- | --------------------- | -------------- |
| {{username}} | all_username   |               | group_user            | web1_user      |



## Ansible Configuration directives

1. Look in $ANSIBLE_CONFIG
2. ./ansible.cfg
3. ~/.ansible.cfg
4. /etc/ansible/ansible.cfg

First config file found wins. Configurations are **not** merged.

Create specific Ansible environment variables by using: $ANSIBLE__configsetting_

Common settings to adjust

|                   | Default | Recommended                           |
| ----------------- | ------- | ------------------------------------- |
| forks             | 5       | 20 - for Production. Adjust as needed |
| host_key_checking | true    | false - for  Dev. environments.       |
| log_path          | _NULL_  | Pick one that makes sense.            |

[All configuration parameteres](https://docs.ansible.com/ansible/intro_configuration.html)

## Demo Time.

1. Empty the .ssh/knownhosts file. 

2. ansible web1 -i inventory_prod -m ping
   ```
   web1 | FAILED => Using a SSH password instead of a key is not possible because Host Key checking is enabled and sshpass does not support this.  Please add this host's fingerprint to your known_hosts file to manage this host.
   ```

3. Create example2/production/ansible.cfg

4. ansible web1 -i inventory_prod -m ping

```
   web1 | success &gt;&gt; {
       "changed": false,
       "ping": "pong"
   }
```

5. The host key is also added to ~/.ssh/knownhosts

6. export ANSIBLE_HOST_KEY_CHECKING=True

7. ansible web1 -i inventory_prod -m ping

   ```
   web1 | FAILED => Using a SSH password instead of a key is not possible because Host Key checking is enabled and sshpass does not support this.  Please add this host's fingerprint to your known_hosts file to manage this host.
   ```

8. ​