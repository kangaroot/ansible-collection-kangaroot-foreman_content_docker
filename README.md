# Ansible collection: kangaroot.foreman_content_docker

Collection for configuring Docker repositories in a Foreman/Katello installation.

The Docker repositories that can be configured from this collection are:

- Docker CE Stable (CentOS base by default)[^1]:
  - EL7 (x86_64)
  - EL8 (x86_64)[^2]
  - EL9 (x86_64)[^2]

- Mirantis Docker EE Stable versions (RHEL base by default):
  - EL7 (x86_64): Latest / 17.03 / 17.06 / 18.03 / 18.09 / 19.03 / 20.10 / 23.0
  - EL8 (x86_64): Latest / 18.09 / 19.03 / 20.10 / 23.0
  - EL9 (x86_64): Latest / 20.10 / 23.0

[^1]: Enabled by default when enabling docker repositories.
[^2]: Enabled by default when enabling a specific Docker release.

## Requirements and Dependencies

Ansible Core 2.13.0 or higher is required for the roles in the collection.

The roles in this collection must be imported by the `kangaroot.foreman` collection. It is possible to directly use the roles in this collection but not recommended.

The `kangaroot.foreman` collection requires the `theforeman.foreman` and `theforeman.operations` collections. To install the required collections, execute:

```shell
ansible-galaxy collection install -r requirements.yml
```

in the collection directory.

## Collection Variables

The `group_vars` directory contains example vars files for the important variables used in the collection roles.

## Usage

The variable `foreman_content_roles` from the `foreman` role in the `kangaroot.foreman` collection contains a list content roles to import.

Add this collection content role to the `foreman_content_roles` list of content roles to import in your playbook project variables.

For example, add the `foreman_content_roles` variable in your `group_vars/foreman.yml` file of your playbook project:

```yaml
# Foreman content roles to include
foreman_content_roles:
  # Package only content
  - kangaroot.foreman_content_docker.foreman_content_docker
  - ...

  # OS content
  - kangaroot.foreman_content_rocky.foreman_content_rocky
  - ...

  # Builtin content
  - kangaroot.foreman_content_builtin.foreman_content_builtin
```

Also ensure that the Docker repositories are enabled and at least one of the Docker release and/or OS related repositories is enabled as well by setting the appropriate enable variables:

```yaml
foreman_enable_docker: true
foreman_enable_docker_ce: true
```

In your playbook, add a task to execute the `kangaroot.foreman.foreman` role:

```yaml
- name: Run kangaroot.foreman roles
  hosts: foreman
  roles:
    - kangaroot.foreman.foreman
```

