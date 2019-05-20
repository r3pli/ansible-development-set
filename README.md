# Ansible development set

This repository contains a ansible starter or ansible bootstrap project for local ansible development.

## Usage

You can download or clone this repository as a ansible development starter kit. All playbooks and other
ansible sources are within the folder src.

If you want to test your written playbooks or roles you can use the wrapper playback:

```console
./playit playbooks/[PLAYBOOK-NAME]/main.yml -i environments/[ENV] -e 'example_var=example_value'
```

The wrapper runs your configuration within the ansible-dev docker image. You could also lint your
configuration with ansible-lint:

```console
./lintit playbooks/[PLAYBOOK-NAME]/main.yml
```

You can test your configuration with molecule:

```console
./testit test [--scenario-name role-xyz]                                                     
```

All your ansible runs are recorded by ARA and can be viewed on Port 9191. Therefor the docker container
should not be removed after execution.

## What and why

This repository contains a multistage environment ansible starter kit for developing playbooks and roles.
The structure within src is:

```
.
+-- ansible.cfg
+-- environments
|   +-- 0_global_vars
|       +-- main.yml
|   +-- dev
|       +-- group_vars
|           +-- all
|               +-- 0_global_vars -> ../../../0_global_vars
|       +-- hosts.yml
|   +-- ...
+-- play
+-- playbooks
|   +-- sample
|       +-- main.yml
|       +-- roles
|           +-- ...
+-- roles
|   +-- ...
```

#### Environments
Different environments are managed and located within the 'environments' folder. Each sub folder should be named as the stage it represents.
Within this subfolder you can place the inventory file, group_vars and host_vars as usual.

#### Global variables
To simulate a type of global and shared variables within ansible and different environments / stages, the 0_global_vars folder is used.
A symlink to 0_global_vars is created in each environment / stage. Since ansible loads the variables in alphabetical order, a 0 has been 
prefixed so that the variables can be overwritten by and within the different environments.

#### Play
Play is a little (work in progress) helper script / wrapper which (should) check different inventories and playbooks against there environment.
This is to prevent configurations from running in the wrong environment. For example: Connecting the production databases to dev.

#### Playbooks
Basic behavior is equal to the default Ansible. For structural clarity and for the 'Play' helper script, another layer has been added. 
Each Playbook is now located in its own directory and this allows:

- easy handling a playbook as submodules
- the exact assignment of associated files
- overwriting (global) roles

#### Roles
The roles directory behaves like default Ansible. Roles can be overwritten within a single Playbook (see 'Playbooks').

## Dependencies

* [github docker-ansible-dev](https://github.com/r3pli/docker-ansible-dev) / [docker-hub ansible-dev](https://hub.docker.com/r/r3pli/ansible-dev)

## License

This project is licensed under the GNU General Public License Version 2 - see the [LICENSE](LICENSE) file for details

## Author Information

* Fabian (r3pli)

