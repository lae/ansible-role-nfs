[![Build Status](https://travis-ci.org/lae/ansible-role-nfs.svg?branch=master)](https://travis-ci.org/lae/ansible-role-nfs)
[![Galaxy Role](https://img.shields.io/badge/ansible--galaxy-nfs-blue.svg)](https://galaxy.ansible.com/lae/nfs/)

lae.nfs
=========

Installs and configures NFS servers and clients.

Role Variables
--------------

| Variable    | Description                   |
|-------------|-------------------------------|
|`nfs_exports`|List of exports to publish.    |
|`nfs_mounts` |List of NFS endpoints to mount.|

Example Playbook
----------------

```
- hosts: nfs-server.local
  become: True
  roles:
    - lae.nfs
  vars:
    nfs_exports:
      - path: /var/log
        exports:
          - "*(ro,sync,nohide)"
      - path: /home
        exports:
          - "10.20.30.0/24(rw)"
      - path: /backups
        exports:
          - "10.20.30.0/24(rw,no_root_squash)"
- hosts: nfs-client.local
  roles:
    - lae.nfs
  vars:
    nfs_mounts:
      - path: /mnt/logs
        remote_host: nfs-server.local
        remote_path: /var/log
        options: ro
      - path: /mnt/home
        remote_host: nfs-server.local
        remote_path: /home
        disabled: yes
      - path: /mnt/backups
        remote_host: nfs-server.local
        remote_path: /backups
```
