Role Name
=========

**This role is currently only supported with Ubuntu**

This role installs a rootless [fluent-bit](https://docs.fluentbit.io/manual/) logging agent and offers limited configuring.
The following fluent-bit input plugins are supported:
* [CPU Log Based Metrics](https://docs.fluentbit.io/manual/pipeline/inputs/cpu-metrics)
* [Docker Log Based Metrics](https://docs.fluentbit.io/manual/pipeline/inputs/docker-metrics)
* [Systemd](https://docs.fluentbit.io/manual/pipeline/inputs/systemd)

**Note**: The Docker Log Based Metrics input plugin does not function without root permissions,
whether the fluent-bit user is in the `docker` group or not (see https://github.com/fluent/fluent-bit/issues/5644).

To monitor docker containers with rootless fluent-bit:
1. Configure the docker log-driver to use `journald`
1. Enable the systemd input plugin supported in this role
1. Apply a `fluent_bit_systemd_filters` like `CONTAINER_NAME=myapp` or `_SYSTEMD_UNIT=docker.service`

Requirements
------------

None.

Role Variables
--------------

### General Role Variables
| variable                  | required  | default           | comments                                  |
|---------------------------|-----------|-------------------|-------------------------------------------|
| fluent_bit_user           | no        | fluent-bit        | Name of the fluent-bit user               |
| fluent_bit_service        | no        | fluent-bit        | Name of the fluent-bit service            |
| fluent_bit_shell          | no        | /bin/bash         | The shell to assign the fluent-bit user   |
| fluent_bit_config_dir     | no        | /etc/fluent-bit   | The fluent-bit configuration directory    |
| fluent_bit_home_dir       | no        | /home/fluent-bit  | The homne to assign the fluent-bit user   |

### fluent-bit cpu input plugin
To enable the cpu input plugin, define the variable `fluent_bit_include_cpu`.
| variable                      | required  | default           | comments                                              |
|-------------------------------|-----------|-------------------|-------------------------------------------------------|
| fluent_bit_include_cpu        | yes/no    | undefined         | Define this variable to enable the cpu input plugin   |
| fluent_bit_cpu_config         | no        | input_cpu.conf    | If you wish to change the name of the cpu config file |
| fluent_bit_cpu_interval_sec   | no        | 60                | The polling interval in seconds                       |

### fluent-bit docker input plugin variables
To enable the docker input plugin, you must populate the variable `fluent_bit_include_docker_containers`
with the docker container ID’s you wish to monitor.
| variable                              | required  | default           | comments                                      |
|---------------------------------------|-----------|-------------------|-----------------------------------------------|
| fluent_bit_include_docker_containers  | yes/no    | undefined         | A space delimited string of container ID’s    |
| fluent_bit_docker_config              | no        | input_docker.conf | If you wish to change the name of the docker config file |
| fluent_bit_docker_interval_sec        | no        | 60                | The polling interval in seconds               |

### fluent-bit systemd input plugin
To enable the systemd input plugin, define the variable `fluent_bit_include_systemd`.
| variable                          | required  | default               | comments                                                  |
|-----------------------------------|-----------|-----------------------|-----------------------------------------------------------|
| fluent_bit_include_systemd        | yes/no    | undefined             | Define this variable to enable the systemd input plugin   |
| fluent_bit_systemd_config         | no        | input_systemd.conf    | If you wish to change the name of the systemd config file |
| fluent_bit_systemd_filters        | no        | undefined             | The fluent-bit Systemd_Filters to apply                   |
| fluent_bit_systemd_read_from_tail | no        | On                    | Skip entries already in the journal                       |

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      vars:
        fluent_bit_include_cpu: true
        fluent_bit_include_docker_containers: 9e92d2f20c7f
        fluent_bit_include_systemd: true
        fluent_bit_systemd_filters:
          - "_SYSTEMD_UNIT=docker.service"
          - "CONTAINER_NAME=myapp"

      roles:
        - fluent-bit

License
-------

GNU GPLv3
