# Vagrant Ansible Packer CentOS-7
A set of [(Ansible)](https://www.ansible.com) scripts around [Packer by HashiCorp](https://www.packer.io/) to build a [CentOS 7](https://www.centos.org) box for [Oracle VM VirtualBox](https://www.virtualbox.org).

- The idea is that the [Kickstart](http://pykickstart.readthedocs.io/en/latest/) file 'ks.cfg' is as simple as possible and all the configuration is done by Ansible.

Props to **Jeff Geerling** aka "***geerlingguy***" who made a great howto on this and is the creator of the original 'packer-rhel' role which is the base of this project.

- [Jeff's HowTo Vagrant and Ansible](http://www.jeffgeerling.com/blog/server-vm-images-ansible-and-packer).
- [geerlingguy/packer-rhel](https://galaxy.ansible.com/geerlingguy/packer-rhel/).

### Prerequisites (on a mac)
```
    brew cask install virtualbox
    brew cask install vagrant
    brew cask install virtualbox-extension-pack
    brew install ansible
    brew install packer
```
If you want the box to upload to Atlas you need to add your Token within the forward quotes to the centos7.json and fill in your Atlas username, or else comment out the whole part. 
```
    {
      "type": "atlas",
      "only": ["virtualbox-iso"],
      "token": "{{`NEW-TOKEN-HERE`}}",
      "artifact": "ATLAS-NAME/centos-7.2",
      "artifact_type": "vagrant.box",
      "metadata": {
        "provider": "virtualbox",
        "created_at": "{{timestamp}}"
      }
    }
```

## HowTo
```
    git clone https://github.com/scorputty/packer-centos-7.git
    cd packer-centos-7
    time packer build centos7.json
```

### Test / develop / troubleshoot ansible from vagrant box like so:
```
    vagrant init virtualbox-centos7 builds/virtualbox-centos7.box
    vagrant up
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
More info on the Ansible part is in the README.md in ***ansible/roles/packer-rhel***
