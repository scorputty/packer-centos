# Vagrant Ansible Packer CentOS-7
A set of [(Ansible)](https://www.ansible.com) scripts around [Packer by HashiCorp](https://www.packer.io/) to build a [CentOS 7](https://www.centos.org) box for [Oracle VM VirtualBox](https://www.virtualbox.org).

- The idea is that the [Kickstart](http://pykickstart.readthedocs.io/en/latest/) file 'ks.cfg' is as simple as possible and all the configuration is done by Ansible.

### Note
Change the variables to your location in 'defaults/main.yml'

### Changes
Mayor overhaul! Switched to chronyd instead of ntpd, also merged the VirtualBox GuestAdditions in to the one main task since I'm only building CentOS for VirtualBox at the moment.

Other notables:

- Migrated to Vagrant Cloud

- You can find the build box here "https://app.vagrantup.com/scorputty/boxes/centos"

- Props to **Jeff Geerling** aka "***geerlingguy***" who made a great howto on this and is the creator of the original 'packer-rhel' role which is the base of this project.

- [Jeff's HowTo Vagrant and Ansible](http://www.jeffgeerling.com/blog/server-vm-images-ansible-and-packer).
- [geerlingguy/packer-rhel](https://galaxy.ansible.com/geerlingguy/packer-rhel/).

## HowTo
These steps explain how this Packer Ansible build is setup.

### Prerequisites (on a mac)
```
    brew cask install virtualbox
    brew cask install vagrant
    brew cask install virtualbox-extension-pack
    brew install ansible
    brew install packer
```

### Clone this repo
```
    git clone https://github.com/scorputty/packer-centos.git
    cd packer-centos
    time packer build centos7-virtualbox.json (or centos7-virtualbox-local.json)
```

### Info
There are two build files, one local and one that does a upload of the image to Vagrant Cloud.
If you want the box to upload to Vagrant Cloud you need to add your ATLAS_TOKEN to your shell ENV (.bash_profile. More info  [here](https://atlas.hashicorp.com/help/packer/artifacts/creating-vagrant-boxes).

### Test / develop / troubleshoot Ansible from within the Vagrant box like so:
```
    vagrant up
    ansible-playbook --user=vagrant --ask-pass -i inventory.yml ansible/site.yml
    vagrant init virtualbox-centos-7 builds/virtualbox-centos-7.box
    vagrant up
    vagrant ssh
    sudo -s
    yum install -y epel-release
    yum install -y ansible
    ansible-playbook -i "localhost," -c local /vagrant/ansible/site.yml
```

## Note
More info on the Ansible part is in the README.md in ***ansible/roles/packer-rhel***
