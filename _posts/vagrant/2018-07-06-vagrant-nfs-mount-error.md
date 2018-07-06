---
layout: "post"
title: "Vagrant requested NFS version or protocol not supported."
categories: vagrant nfs
---

I've had this error a few times, when trying to build a VM, and always forget the cause as its totally un-related to the error message as shown below:
    
    The following SSH command responded with a non-zero exit status.
    Vagrant assumes that this means the command failed!

    set -e
    mkdir -p /home/vagrant/projects
    mount -o vers=3,udp 192.168.33.1:/Users/xxx/Projects/simplebox-test/simpleboxtest /home/vagrant/projects
    if command -v /sbin/init && /sbin/init --version | grep upstart; then
    /sbin/initctl emit --no-wait vagrant-mounted MOUNTPOINT=/home/vagrant/projects
    fi

    Stdout from the command:

    Stderr from the command:

    stdin: is not a tty
    mount.nfs: requested NFS version or transport protocol is not supported
    
Basically something clobbers my `/etc/hosts` file and removes most of the entries including the `127.0.0.1  localhost` entry, which seems to be the cause of this problem.
I now keep a backup of the hosts file and replace the real one when it happens.
This happened again today after an upgrade of VirtualBox, and some packer builds.