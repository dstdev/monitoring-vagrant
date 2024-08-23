

If Vagrant is installed:

```bash
vagrant up
```

Once the vm is up, ssh into it `vagrant ssh`

```
# Got to the synced directory under /vagrant
cd /vagrant
# Install git
sudo yum install git
# Install the codeready builder
sudo dnf config-manager --set-enabled crb
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
# In order for ansible to target this vm, we need an inventory
echo "localhost" >> inventory.cfg
# Run the plabyook
# -b # Become a privileged user. Default is to use sudo
# -c local # Run via a local connection instead of ssh. This can be faster
# -i inventory.cfg # Must define the inventory file in the absence of a ansible.cfg
# monitoring.yaml # The monitoring install playbook
ansible-playbook -b -c local -i inventory.cfg monitoring.yaml
# Stop the firewall to access the web interfaces
systemctl stop firewalld
systemctl disable firewalld
```
