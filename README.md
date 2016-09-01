# hands-on-ansible
Stuff from the course: Hands-on-Ansible.

## Getting up and running:

`vagrant up`

`VBoxManage list runningvms`

`vagrant ssh acs`
* `sudo apt-get install ansible sshpass`

`vagrant ssh web`
* `sudo yum install epel-release`
* `sudo yum install ansible`

### Build Ansible from source
`vagrant ssh db`
* `sudo yum install -y gcc make libffi-devel python-setuptools python-devel`
* `sudo easy_install pip`
* `sudo pip install ansible`

### Excercise 1
`vagrant ssh acs`
* ```vagrant@acs:/vagrant/exercise1$ ansible 192.168.33.20 -i inventory -u vagrant -m ping -k
SSH password:
192.168.33.20 | FAILED => to use the 'ssh' connection type with passwords, you must install the sshpass program```
* `ssh vagrant@192.168.33.20` -- add the sshkey
* Repeat the Vagrant command.

```json
192.168.33.20 | success >> {
    "changed": false,
    "ping": "pong"
}

192.168.33.30 | success >> {
    "changed": false,
    "ping": "pong"
}```

* Run a command on a remote machine: `vagrant all -i inventory -u vagrant -m command -a "/sbin/yum update - y"`
  * 'all' => The name of the host to connect. 'all' means all hosts in the inventory.
  * -i => Inventory File
  * -u => User
  * -m => Ansible module
  * -a => Attribute to the module (for the command module. This is the command to run.)
  * -v(vv) => Be verbose, very verbose, or very,   very verbose

The `command` module executes commands inside the Python context. The `shell` module executes commands in a shell, and thus has access to environment variables.
