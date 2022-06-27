Role Name
=========

**This role is currently only supported with Ubuntu**

This role installs `docker-ce` per Docker’s [documentation](https://docs.docker.com/engine/install/ubuntu/),
and `docker-compose` directly from the official [repository](https://github.com/docker/compose/releases).
It also has limited ability to configure `/etc/docker/daemon.json` by setting the docker log-driver.

Requirements
------------

None.

Role Variables
--------------

| Variable                  | Required  | Default       | Comments                                  |
|---------------------------|-----------|---------------|-------------------------------------------|
| docker_config_dir         | no        | /etc/docker   | It’s not recommended you change this      |
| docker_daemon_config      | no        | daemon.json   | It’s not recommended you change this      |
| docker_daemon_log_driver  | no        | local         | See the official [docs](https://docs.docker.com/config/containers/logging/configure/) |
| docker_compose_version    | no        | 2.6.1         |                                           |
| docker_compose_arch       | no        | linux-x86_64  | See the docker-compose release [page](https://github.com/docker/compose/releases) |
| docker_compose_checksum   | no        | sha256:ed79398562f3a80a5d8c068fde14b0b12101e80b494aabb2b3533eaa10599e0f |
| docker_compose_file       | no        | /usr/local/bin/docker-compose | Where to install          |

Dependencies
------------

None.

Example Playbook
----------------

To install docker, docker-compose, and configure the docker log-driver to jounrald:

    - hosts: servers
      vars:
        docker_daemon_log_driver: journald
      roles:
         - docker

License
-------

GNU GPLv3
