# Vagrant Ansible Packer CentOS-7
A set of (Ansible) scripts around HashiCorp's Packer to build a CentOS-7 box for Vagrant or VirtualBox.

- The idea is that the kickstart file 'ks.cfg' is as simple as possible and all the configuration is done by Ansible.

Props to Jeff Geerling's aka "geerlingguy" who's code and howto is the base of this project.

[geerlingguy/packer-rhel](https://galaxy.ansible.com/geerlingguy/packer-rhel/).
[Jeff's howto Vagrant and Ansible](http://www.jeffgeerling.com/blog/server-vm-images-ansible-and-packer).

### Prerequisites (on a mac)
```
    brew cask install virtualbox
    brew cask install vagrant
    brew cask install virtualbox-extension-pack
    brew install ansible
    brew install packer
```

## Howto
```
    git clone https://github.com/scorputty/packer-centos-7.git
    cd packer-centos-7
    time packer build centos7.json
```

### Test / develop / troubleshoot ansible from vagrant box like so:
```
    vagrant init virtualbox-centos7 builds/virtualbox-centos7.box
    vagrant ssh
    sudo -s
    yum install -y ansible
    ansible-playbook -i "localhost," -c local /vagrant/ansible/main.yml
```
You might want to comment out:
```
    - include: virtualbox.yml
      when: virtualbox_check.stat.exists
```  

## Note
More info on the Ansible part is in the README.md in ansible/roles/packer-rhel
