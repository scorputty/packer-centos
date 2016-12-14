# Vagrant Ansible Packer CentOS-7
A set of [(Ansible)](https://www.ansible.com) scripts around [Packer by HashiCorp](https://www.packer.io/) to build a [CentOS 7](https://www.centos.org) box for [Oracle VM VirtualBox](https://www.virtualbox.org).

- The idea is that the [Kickstart](http://pykickstart.readthedocs.io/en/latest/) file 'ks.cfg' is as simple as possible and all the configuration is done by Ansible. Also Ansible is ran from your OSX Laptop, it's not installed on the VM.

Other notables:

- btrfs
- no perl

- You can find the build box here "https://atlas.hashicorp.com/scorputty/boxes/centos-7.2"

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
If you want the box to upload to Atlas you need to add your ATLAS_TOKEN and ATLAS_BUILD_SLUG to your shell ENV, or else comment out the whole part. Setting up the atlas env stuff is explained [here](https://vagrantcloud.com/help/packer/builds/build-environment).
```
    {
      "type": "atlas",
      "only": ["virtualbox-iso"],
      "token": "{{user `atlas_token`}}",
      "artifact": "{{user `atlas_build_slug`}}",
      "artifact_type": "vagrant.box",
      "metadata": {
        "provider": "virtualbox",
        "created_at": "{{timestamp}}",
        "version": ""
      }
```

## HowTo
```
    git clone https://github.com/scorputty/packer-centos.git
    cd packer-centos
    time packer build centos7.json
```

### Test / develop / troubleshoot ansible from vagrant box like so:
```
    vagrant init virtualbox-centos-7.2.box builds/virtualbox-centos-7.2.box
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
