# ctorgalson.get_docker (ctorgalson/ansible-role-get-docker)

This role retrieves and runs the script from https://get.docker.com. It offers
three install strategies for different situations:

  1. install-only (when the docker binary can not be located),
  2. commit-hash based (when the install script matches a configurable commit
     hash),
  3. force (always run the install script).

The role attempts to provide a small amount of additional security over simply
downloading and installing the role by comparing the script downloaded from
https://get.docker.com, and [the version hosted on github.com](https://raw.githubusercontent.com/docker/docker-install/master/install.sh).

If the diff of the two files is more than a single line, or if the single line
does not match the `$SCRIPT_COMMIT_SHA` assignment line, the role will not
attempt the install.

There are other, more sophisticated (and much more widely-used) Ansible roles
for installing Docker. Unless you have specific reasons for using the shell
script installer from https://get.docker.com you should probably use one of
those instead. For example, I've used both of the following roles with good
results:

  - [geerlingguy.docker](https://galaxy.ansible.com/geerlingguy/docker)
  - [robertdebock.docker](https://galaxy.ansible.com/robertdebock/docker)

## Requirements

The only requirement of the role is that (per Docker's advice), it should only
be run on machines **if**:

  - the machine does not already have `docker-ce` installed,
  - the machine has previously had `docker-ce` installed by https://get.docker.com.

If you're trying to use this role to install `docker-ce` alongside other
`docker*` packages, please be sure you know what you're doing.

## Role Variables

### Defaults

| Variable name              | Default value | Required | Description |
|----------------------------|---------------|----------|-------------|
| `gd__force_install_script` | `false`       | no       | Forces the installer to run even if Docker has already been installed. |
| `gd__script_commit_sha`    | `undefined`   | no       | When set, the installer will only run if its `$SCRIPT_COMMIT_SHA` variable is set to this value. |

### Vars

| Variable name        | Default value | Required | Description |
|----------------------|---------------|----------|-------------|
| `gd__bin_path`       | `/usr/bin/docker`        | yes | The path the role should search to see if Docker is already installed. |
| `gd__get_docker_com` | `https://get.docker.com` | yes | The URL of the downloadable Docker install script. |
| `gd__install_sh_url` | `https://raw.githubusercontent.com/docker/docker-install/master/install.sh` | yes | The URL of the github.com-hosted, pre-build get.docker.com install script. Compared with get.docker.com script for increased install safety. |

## Dependencies

The role does not depend on any other roles.

## Example Playbook

The role will install Docker once given this configuration. In concert with
unattended upgrades, this will do for many situations, but it's up to you to
keep it up-to-date. See the [Variables](#variables) section above for more info.

    ---
    - name: Install Docker.
      hosts: all
      gather_facts: false
      tasks:
        - name: "Include ctorgalson.get_docker"
          ansible.builtin.include_role:
            name: "ctorgalson.get_docker"

## License

GPL-3.0-only

## Author Information

Christopher Torgalson
