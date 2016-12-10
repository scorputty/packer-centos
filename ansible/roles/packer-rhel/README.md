# Ansible Role: Packer CentOS Configuration for Vagrant VirtualBox

This role configures CentOS (either minimal or full install) in preparation for it to be packaged as part of a .box file for Vagrant/VirtualBox deployment using [Packer](http://www.packer.io/).

It's based heavily on Jeff Geerling's [Galaxy](https://galaxy.ansible.com/geerlingguy/packer-rhel/). With the difference that this role is VirtualBox only. One other thing, I've tried to do as much as possible in the role instead of in the Kickstart file. This makes the whole process more transparent.

## Requirements

Prior to running this role via Packer, you need to make sure Ansible is installed via a shell provisioner, and that preliminary VM configuration (like adding a vagrant user to the appropriate group and the sudoers file) is complete, generally by using a Kickstart installation file (e.g. `ks.cfg`) with Packer. An example array of provisioners for your Packer .json template would be something like:

    "provisioners": [
      {
        "type": "shell",
        "execute_command": "echo 'vagrant' | {{.Vars}} sudo -S -E bash '{{.Path}}'",
        "script": "scripts/ansible.sh"
      },
      {
        "type": "ansible-local",
        "playbook_file": "ansible/main.yml",
        "role_paths": [
          "/etc/ansible/packer-rhel",
        ]
      }
    ],

The files should contain, at a minimum:

**scripts/ansible.sh**:

    #!/bin/bash -eux
    # Install EPEL repository.
    yum -y install epel-release
    # Install Ansible.
    yum -y install ansible

**ansible/main.yml**:

    ---
    - hosts: all
      sudo: yes
      gather_facts: yes
      roles:
        - scorputty.packer-rhel

You might also want to add another shell provisioner to run cleanup, erasing free space using `dd`, but this is not required (it will just save a little disk space in the Packer-produced .box file).

If you'd like to add additional roles, make sure you add them to the `role_paths` array in the template .json file, and then you can include them in `main.yml` as you normally would. The Ansible configuration will be run over a local connection from within the Linux environment, so all relevant files need to be copied over to the VM; configuratin for this is in the template .json file. Read more: [Ansible Local Provisioner](http://www.packer.io/docs/provisioners/ansible-local.html).

## Role Variables

None.

## Dependencies

None.

## Example Playbook

    - hosts: all
      roles:
        - { role: scorputty.packer-rhel }

## License

MIT / BSD

## Author Information

This role was created in 2016 by [Stef Corputty].
