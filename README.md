# vagrant packer-centos-7
Packer image build code for vagrant ansible virtualbox

- The idea is that the kickstart file is as simple as possible and all the configuration is done by Ansible.

Props to geerlingguy the ansible guru and Joyent's packer-centos-7 repo for inspiration

### prerequisites (on a mac)
```
brew cask install virtualbox
brew cask install vagrant
brew cask install virtualbox-extension-pack
brew install ansible
brew install packer
```

### howto
```
time packer build centos7.json
```

### test / develop / troubleshoot ansible from vagrant box like so:
```
$ vagrant init virtualbox-centos7 builds/virtualbox-centos7.box
$ vagrant ssh
$ sudo -s
$ yum install -y ansible
$ ansible-playbook -i "localhost," -c local /vagrant/ansible/main.yml
```
