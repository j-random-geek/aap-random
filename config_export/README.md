# Controller configuration export playbooks
This is a sample just to show AAP config exports in two formats - as a single YAML file or as a filetree. This might be useful for verifying that your Configuration-as-Code has applied the same across a series of AAP clusters. Or it might not be useful at all.

This has been tested against AAP 2.4 (Controller 4.4.0) with version 4.4.0 of the ansible.controller collection.

## Dependencies
Collections:
- ansible.controller

Python:
- awxkit


## Variables
Most of these are contained in vars.yml:
| Variable | Required | Default | Comment |
|:--------:|:--------:|:-------:|:-------:|
| `controller_admin_user` | Y | | Username for a superuser on the AAP Controller |
| `controller_admin_password` | Y | | Password for the above superuser |
| `validate_certs` | N | N | Whether or not to perform SSL certificate validation |
| `config_dump_path` | N | `.` | The location where the config yaml file or the filetree directory will be created |

## Usage
First, update the inventory (`inventory.yml`) hosts and variables for your environment. Sensitive variables should, naturally, be vaulted.

You should then be able to run one of the playbooks as follows.

The `export-yaml.yml` playbook creates a single YAML file containing the Controller configuration. It's not that easy to read - no whitespace, etc. It creates a file whose filename contains a timestamp and the name of the controller the export came from (i.e. `config_dump_path/YYYYMMDD-HHMMSS-controller_hostname-config.yml`):

`ansible-playbook -i inventory.yml --ask-vault-pass -e config_dump_path=~/ export-yaml.yml`

The `export-filetree.yml` playbook creates a directory tree, named `config_dump_path/YYYYMMDD-HHMMSS-controller_hostname-config/` containing the Controller configuration. This one is easier for human eyes to make sense of but sometimes there are odd things that aren't quite right. It can be a good source of information for configuration-as-code information:

`ansible-playbook -i inventory.yml --ask-vault-pass -e config_dump_path=~/ export-filetree.yml`

