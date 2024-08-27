# Monitoring Test Environment

## Setup

### With Vagrant
If Vagrant is installed:

```bash
vagrant up
```
### Without Vagrant

If you really don't want to use vagrant, there are some extra packages and things needed

```bash
sudo dnf install git
sudo dnf config-manager --set-enabled crb
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```

## Running the playbook
Once the vm is up, ssh into it `vagrant ssh`

```
# Got to the synced directory under /vagrant
cd /vagrant
# Create a python virtual environment to install ansible and other 
# dependencies into
python3 -m venv venv
# Activate the environment
source venv/bin/activate
# Install ansible and jmespath
# jmespath is required for adding dashboards to grafana
pip install ansible jmespath
# Install extra roles needed
ansible-galaxy install -r requirements.yaml
# Clone the dst ansible roles repository
git clone https://github.com/dstdev/ansible-roles roles
# Run the plabyook
# -b # Become a privileged user. Default is to use sudo
# -i inventory.cfg # Must define the inventory file in the absence of a ansible.cfg
# monitoring.yaml # The monitoring install playbook
ansible-playbook -b -c local -i inventory.cfg monitoring.yaml
```


## Accessing Dashboards

Find the ip address of your VM

```console
[vagrant@rocky9 vagrant]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:60:ad:0a brd ff:ff:ff:ff:ff:ff
    altname enp0s5
    altname ens5
    inet 192.168.121.166/24 brd 192.168.121.255 scope global dynamic noprefixroute eth0
       valid_lft 2321sec preferred_lft 2321sec
```

Now open your web browser to access ports 3000 (grafana), 9090 (prometheus)
