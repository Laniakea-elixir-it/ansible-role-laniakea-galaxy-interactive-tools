Laniakea Interactive Tools
==========================

This role is used to configure [Interactive Tools](https://training.galaxyproject.org/training-material/topics/admin/tutorials/interactive-tools/tutorial.html)
on Galaxy. Currently, it has been tested on Galaxy 21.09.


Role Variables
--------------

### Galaxy gie-proxy variables
| Variable           | Description                                                         | Default                                      |
| ------------------ | ------------------------------------------------------------------- | -------------------------------------------- |
| gie_proxy | List containing variables used by the [usegalaxy-eu.gie_proxy](https://github.com/usegalaxy-eu/ansible-gie-proxy) role | // |
| dir                | Directory where the gie-proxy is installed                          | /home/galaxy/galaxy/gie-proxy/proxy          |
| git_version        | Git reference to clone                                              | main                                         |
| setup_nodejs       | Whether to install Node.js, options are `package` and `nodeenv`     | nodeenv                                      |
| virtualenv_command | Command to create virtualenv when using nodeenv method              | /usr/bin/python3 -m virtualenv               |
| nodejs_version     | Version of Node.js to install if using nodeenv method               | "16.14.0"                                    |
| virtualenv         | Path of virtualenv into which nodeenv/Node.js/npm will be installed | /home/galaxy/galaxy/gie-proxy/venv           |
| setup_service      | Whether to configure the proxy as a service, only option is systemd | systemd                                      |
| sessions_path      | Path of Interactive Tools sessions map               | "{{ galaxy_mutable_data_dir }}/interactivetools_map.sqlite" |
| gie_proxy_port     | Port where gie-proxy is listening                                   | 8000                                         |


### Galaxy variables
| Variable                          | Description                            | Default                                             |
| --------------------------------- | -------------------------------------- | --------------------------------------------------- |
| laniakea_galaxy_user              | Name of the user running galaxy        | galaxy                                              |
| galaxy_install_path               | Galaxy base directory                  | /home/galaxy/galaxy                                 |
| laniakea_galaxy_server_dir        | Galaxy server directory                | "{{ galaxy_install_path }}/server"                  |
| laniakea_galaxy_config_dir        | Galaxy config directory                | "{{ galaxy_install_path }}/config"                  |
| laniakea_galaxy_config_file       | galaxy.yml config file path            | "{{ laniakea_galaxy_config_dir }}/galaxy.yml"       |
| galaxy_mutable_data_dir           | Galaxy var direcotry                   | /home/galaxy/galaxy/var                             |
| galaxy_tool_conf_interactive_path | Path to tool_conf_interactive.xml file | "{{ laniakea_galaxy_config_dir }}/tool_conf_interactive.xml" |
| export_dir                        | Export dir where data and docker images are stored | /export                                 |
| interactive_tools_galaxy_config.galaxy | List of key, value pairs added in galaxy.yml | //                                       |
| galaxy_config_templates           | List containing src and dest for galaxy config templates | //                                |

#### interactive_tools_galaxy_config.galaxy variables
These are the variables stored in the interactive_tools_galaxy_config.galaxy variable, as written in the galaxy.yml config file
| Variable                  | Description                           | Default                                         |
| ------------------------- | ------------------------------------- | ----------------------------------------------  |
| job_config_file           | Path to job_conf.xml                  | "{{ laniakea_galaxy_config_dir }}/job_conf.xml" |
| interactivetools_enable   | Enables interactive tools             | true                                            |
| interactivetools_map      | Path to interactive tools session map | "{{ gie_proxy.sessions_path }}"                 |
| galaxy_infrastructure_url | Galaxy infrastructure URL             | "http://{{ inventory_hostname }}/galaxy/"       |

### Nginx variables
| Variable       | Description                            | Default    |
| -------------- | -------------------------------------- | ---------- |
| nginx_conf_dir | Path of configurations files for nginx | /etc/nginx |

### Pulsar variables
| Variable                   | Description                          | Default                                                  |
| -------------------------- | ------------------------------------ | -------------------------------------------------------- |
| pulsar_config_path         | Path to pulsar config file           | "{{ laniakea_galaxy_config_dir }}/pulsar_app.yml"        |
| galaxy_job_working_dir     | Path to Galaxy job working directory | "{{ export_dir }}/galaxy/database/job_working_directory" |
| galaxy_tool_dependency_dir | Galaxy tool_deps directory           | "{{ export_dir }}/tool_deps"                             |

### Interactive tools variables
| Variable                     | Description                          | Default                                                          |
| ---------------------------- | ------------------------------------ | ---------------------------------------------------------------- |
| interactive_tools            | List of interactive tools installed  | 'bam_iobio','jupyter_notebook','rstudio','vcf_iobio'             |
| interactive_dir              | Interactive tools config dir         | "{{ laniakea_galaxy_server_dir }}/tools/interactive"             |
| interactivetool_manager_file | Interactivetool.py manager path      | "{{ laniakea_galaxy_server_dir }}/lib/galaxy/managers/interactivetool.py" |
| rstudio_interactive_file     | interactivetool_rstudio.xml path     | "{{ interactive_dir }}/interactivetool_rstudio.xml"              |
| jupyter_interactive_file     | interactivetool_jupyter_notebook.xml | "{{ interactive_dir }}/interactivetool_jupyter_notebook.xml"     |
| pulsar_kill_util_file | kill.py pulsar manager path | "{{ galaxy_install_path }}/venv/lib/python3.6/site-packages/pulsar/managers/util/kill.py" |


Dependencies
------------

Required roles:
* geerlingguy.docker, version 2.6.0
* usegalaxy_eu.gie_proxy, version 0.0.2

Example Playbook
----------------

```yml
---
- name: Galaxy Interactive Tools
  hosts: all
  roles:
    - role: "/path/to/ansible-role-interactive-tools/"
      become: true
```

License
-------

Author Information
------------------

[Daniele Colombo](https://github.com/dacolombo)