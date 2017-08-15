# Ansible Role: Packer CentOS Configuration for Vagrant VirtualBox

This role configures CentOS (either minimal or full install) in preparation for it to be packaged as part of a .box file for Vagrant/VirtualBox deployment using [Packer](http://www.packer.io/).

It's based heavily on Jeff Geerling's aka "geerlingguy" [Galaxy role packer-rhel](https://galaxy.ansible.com/geerlingguy/packer-rhel/). The difference that this role is VirtualBox only. One other thing, I've tried to do as much as possible in the role instead of in the Kickstart file. This makes the whole process more transparent.

### Note
Change the variables to your location in 'defaults/main.yml'

### Changes
Mayor overhaul! Switched to chronyd instead of ntpd, also merged the VirtualBox GuestAdditions in to the one main task since I'm only building CentOS for VirtualBox at the moment.
