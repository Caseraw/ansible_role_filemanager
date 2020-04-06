# Ansible role filemanager

Managing files, directories and file links.

[![Build Status](https://travis-ci.org/Caseraw/role_file_manager.svg?branch=master)](https://travis-ci.org/Caseraw/role_file_manager) [<img src="https://img.shields.io/ansible/role/47700">](https://galaxy.ansible.com/caseraw/role_file_manager) [<img src="https://img.shields.io/ansible/role/d/47700">](https://galaxy.ansible.com/caseraw/role_file_manager) [<img src="https://img.shields.io/ansible/quality/47700">](https://galaxy.ansible.com/caseraw/role_file_manager)

- [Ansible role filemanager](#ansible-role-filemanager)
  - [License](#license)
  - [Author Information](#author-information)
  - [Requirements](#requirements)
  - [Dependencies](#dependencies)
  - [Compatibility](#compatibility)
  - [Role Variables](#role-variables)
  - [Example Playbook](#example-playbook)
  - [Useful shell commands](#useful-shell-commands)
  - [Additional documentation resources](#additional-documentation-resources)
  - [Testing with Molecule](#testing-with-molecule)
  - [CI/CD with Travis CI](#cicd-with-travis-ci)
  - [Useful links](#useful-links)

## License

MIT / BSD

## Author Information

- Made and maintained by: [Kasra Amirsarvari](https://www.linkedin.com/in/caseraw)
- Ansible Galaxy community author: <https://galaxy.ansible.com/caseraw>
- Dockerhub community user: <https://hub.docker.com/u/caseraw>

## Requirements

- Ensure sufficient privileged permissions to manage files, directories, symlinks and hardlinks.

## Dependencies

N/A

## Compatibility

Compatible with the following list of operating systems:

- CentOS 7
- CentOS 8
- RHEL 7.x
- RHEL 8.x

## Role Variables

| Variable name | Description |
|---------------|-------------|
| role_file_manager_file_list | Combined list of other lists that start with the name `role_file_manager_file_list`. Each list contains files and folders to manage. |

> Due to the flexibility to provide custom templates, it is possible to define custom variables that can be used in the jinja templates. This can be very useful to generate custom dynamic files from templates including jinja logic.
>
> Please to understand that this is best used for content that does not require a specific service restart. This role does not provide options for a handler or any other type of restart/reload task for any service or application. It is best used in scenarios where adding, modifying or removing a file/directory/symlink (in place) does the job.
>
> Be cautious when using the `type: touch` as it is not idempotent. It will create an empty file if it doesn't exists and touch/update the file attributes when it does exists. Although strange, it could be useful for rare scenarios.

## Example Playbook

```yaml
---
- name: Managing files, directories and file links
  become: False
  gather_facts: False
  tasks:
    - import_role:
        name: ansible_role_filemanager
      vars:
        role_file_manager_file_list_default:
          - type: directory
            state: present
            dest: /tmp/example.dir
            owner: root
            group: root
            mode: '0755'
          - type: file
            state: present
            src: example.file
            dest: /tmp/example.file
            owner: root
            group: root
            mode: '0644'
          - type: template
            state: present
            src: example.template.j2
            dest: /tmp/example.template
            owner: root
            group: root
            mode: '0644'
          - type: link
            state: present
            src: /tmp/example.file
            dest: /tmp/symlink_to_example.file
            owner: root
            group: root
            mode: '0644'
          - type: hard
            state: present
            src: /tmp/example.template
            dest: /tmp/hardlink_to_example.template
        # Be cautious with 'type: touch', it's not idempotent
        # - type: touch
        #   state: present
        #   dest: /tmp/example.touch.file.tmp
        #   owner: root
        #   group: root
        #   mode: '0644'

...
```

## Useful shell commands

N/A

## Additional documentation resources

N/A

## Testing with Molecule

This role is locally tested with the use of [Molecule](https://molecule.readthedocs.io/en/latest/), the configuration is located at: [molecule/default](molecule/default).  
The Molecule tests are run (using the [docker driver](https://molecule.readthedocs.io/en/latest/configuration.html#docker)) on [Dockerhub images](https://hub.docker.com/u/caseraw) built for this purpose:

- [CentOS](https://hub.docker.com/r/caseraw/ansible-molecule-centos)
- [Fedora](https://hub.docker.com/r/caseraw/ansible-molecule-fedora)

## CI/CD with Travis CI

This role uses [Travis CI](https://travis-ci.org/) to run online tests with the use of [Molecule](https://molecule.readthedocs.io/en/latest/) and pushes notifications to import the role into [Ansible Galaxy](https://galaxy.ansible.com/) once the tests are successful. The Travis CI configuration is located at the root of the Ansible role [.travis.yml](.travis.yml)

## Useful links

- GitHub repository: <https://github.com/Caseraw/ansible_role_filemanager>
- Travis CI build status: <https://travis-ci.org/Caseraw/ansible_role_filemanager>
- Ansible Galaxy role: <https://galaxy.ansible.com/caseraw/ansible_role_filemanager>
