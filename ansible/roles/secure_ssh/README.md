Role Name
=========

**This role is currently only supported with Ubuntu**

This role configures SSH as below:
* Creates a wheel user in the `sudo` group
* Adds the wheel user to the sudoers
* Adds a public key to the wheel user’s authorized keys
* Disables SSH password authentication
* Disables root SSH logins

Requirements
------------

None.

Role Variables
--------------

| variable                  | required  | default   | comments                                      |
|---------------------------|-----------|-----------|-----------------------------------------------|
| wheel_user                | yes       | undefined | The name of your wheel user                   |
| wheel_user_ssh_public_key | yes       | undefined | The public SSH key to login as the wheel user |
| wheel_user_shell          | no        | /bin/bash | The shell to grant the wheel user             |

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      vars:
        wheel_user: jump
        wheel_user_ssh_public_key: ssh-rsa AAAAB3NzaC1yc2E...Q02P1Eamz/nT4I3 user@localhost
      roles:
         - secure_ssh

License
-------

GNU GPLv3
