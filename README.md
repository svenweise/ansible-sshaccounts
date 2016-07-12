ansible-shellaccounts
=======================

A role for managing ssh shellaccounts


Actions
-------

- Creates users additional groups with sudo files
- Creates users with personal group and sudo files
- Sync SSH-Keys


Requirements
------------

None


Important note
--------------

All additional groups (variable: unixgroups) have to have a sudoers file (default filename: groupname).
If you don't want sudorights at group-level yust leave an empty file.


Role Variables
--------------

# Where to find the ssh-key and sudoers files and the default unixgroup for the additional groups
sshkeypath: "sshkeys/"
sudoerspath: "sudoers/"
defaultunixgroup: ['users']

# List of additional unixgroups (configured at group_vars/host_vars or playbook)
# Mandatory
unixgroups: []

All these groups have to have a sudoersfile (even if empty) at sudoerspath

# List of users (configured at group_vars/host_vars or playbook)
# Mandatory
users: []

# Mandatory parameter for unixgroups are:
- name: "string"

# Mandatory parameters for users are:
- name: "string"
- coment: "string"

Example Playbook
----------------

---
- hosts: all
  become: true
  roles:
    - ansible-shellaccounts


Example variables file
----------------------

(At group_vars/host_vars or at playbook)

# Full example
---
users:
  - name:     "sven"
    comment:  "Sven Weise"
    uid:      "42"
    gid:      "42"
	group:    "sven"
	groups:
	  - admin
	  - ssh
	home:     "/home/sven"
	shell:    "/bin/sh"
	password: "$passwordhash$"
	keyfile:  "id_dsa.sven.pub"
	sudoers:  "svens-sudoers"
	state:    "absent"

unixgroups:
  - name: "admin"
    gid:  "666"
  - name: "ssh"


# Minimal example
---
users:
  - name:     "sven"
    comment:  "Sven Weise"

unixgroups: []

The ssh-key file, if not given, will be searched at sshkeypath as username.pub


Some notes
----------

If you restrict SSH access via allowed_groups at sshd_config I would recomend you set a dependency at meta/main.yml:
dependencies:
  - { role: ansible-sshd }


License
-------

BSD

Author Information
------------------

This role was created in 2016 by [Sven Weise](https://github.com/svenweise)
