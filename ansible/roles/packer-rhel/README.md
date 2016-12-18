# Ansible Role: Packer CentOS Configuration for Vagrant VirtualBox

This role configures CentOS (either minimal or full install) in preparation for it to be packaged as part of a .box file for Vagrant/VirtualBox deployment using [Packer](http://www.packer.io/).

It's based heavily on Jeff Geerling's aka "geerlingguy" [Galaxy role packer-rhel](https://galaxy.ansible.com/geerlingguy/packer-rhel/). The difference that this role is VirtualBox only. One other thing, I've tried to do as much as possible in the role instead of in the Kickstart file. This makes the whole process more transparent.

### Note
Change the variables in 'defaults/main.yml'
```
    timezone: Europe/Amsterdam
    ntp_server: [0.nl.pool.ntp.org, 1.nl.pool.ntp.org]
```

### The following main tasks are executed
```
    - name: Fix slow DNS (adapted from Bento).
    - name: Bug fixing ifcfg-lo errors network loopback
    - name: Remove RedHat interface persistence.
    - name: System tuning
    - name: Disable unnecessary services
    - name: Restart network service (explicitly).
    - name: Get the current kernel release.
    - name: Ensure necessary packages are installed.
    - name: Check if VirtualBox is running the guest VM.
    - name: Remove unneeded packages.
    - name: Remove surplus kernels
    - name: Clean up yum.
    - name: Remove files
    - name: Clean yum caches
    - name: Set timezone
    - name: Copy the ntp.conf template file
    - name: Ensure NTP is running and enabled as configured.
    - name: Configure SSH daemon.
    - name: Get Vagrants public key.
```

### A separate included task is added for virtualbox guest additions
```
    - name: Get VirtualBox version.
    - name: Mount VirtualBox guest additions ISO.
    - name: Run VirtualBox guest additions installation.
    - name: Unmount VirtualBox guest additions ISO.
    - name: Delete VirtualBox guest additions ISO.
```
