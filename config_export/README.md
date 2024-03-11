# Controller configuration export playbooks
This is a sample just to show AAP config exports in three formats - as a single YAML file, as a filetree, or as a flattened filetree. For more details on the filetree formats, see the [README for `infra.controller_configuration.filetree_create` role](https://github.com/redhat-cop/controller_configuration/tree/devel/roles/filetree_create). These might be useful for verifying that your Configuration-as-Code has applied the same across a series of AAP clusters. Or it might not be useful at all.

This has been tested against AAP 2.4 (Controller 4.5.0) with the collections listed below. Note that the flattened filetree is not available in older versions of the `controller_configuration` collection, and the flattened filetree **is now the default behaviour**. See the Variables section, below, for details on disabling it.

## Dependencies
Collections:
- `ansible.controller` >= 4.4.0
- `infra.controller_configuration` >= 2.6.0

Python:
- `awxkit`


## Variables
Most of these are contained in vars.yml:
| Variable | Type | Required | Default | Comment |
|:---------|:-----|:--------:|:--------|:--------|
| `controller_admin_user` | String | Y | | Username for a superuser on the AAP Controller |
| `controller_admin_password` | String | Y | | Password for the above superuser |
| `validate_certs` | Boolean | N | N | Whether or not to perform SSL certificate validation |
| `config_dump_path` | String | N | `.` | The location where the config yaml file or the filetree directory will be created |
| `export_filetree_flatten` | Boolean | N | true | Whether or not to create a flattened filetree. Used only in `export-filetree.yml` |
| `controller_port` | Integer | N | - | Enables the use of a port other than 443 for https connection to the Automation Controller. Useful for the containerized version of AAP, and expected to be set in the inventory |

In addition, each playbook has a Boolean variable to control logging of potentially-sensitive information (credentials, etc):

| Playbook | Variable | Default | Comment |
|:---------|:---------|:-------:|:--------|
| `export_filetree.yml` | `export_filetree_secure_logging` | true | Set to false to disable `no_log` on tasks that handle sensitive information |
| `export_yaml.yml` | `export_yaml_secure_logging` | true | Set to false to disable `no_log` on tasks that handle sensitive information |


## Usage
First, update the inventory (`inventory.yml`) hosts and variables for your environment. Sensitive variables should, naturally, be vaulted.

You should then be able to run one of the playbooks as follows.

The `export-yaml.yml` playbook creates a single YAML file containing the Controller configuration. It's not that easy to read - short on whitespace, etc. It creates a file whose filename contains a timestamp and the name of the controller the export came from (i.e. `config_dump_path/YYYYMMDD-HHMMSS-controller_hostname-config.yml`):

`ansible-playbook -i inventory.yml --ask-vault-pass -e config_dump_path=~/ export-yaml.yml`

The `export-filetree.yml` playbook creates a directory tree containing the Controller configuration. This one is easier for human eyes to make sense of and can be a good source of information for configuration-as-code information.  The filetree comes in two forms, and the directory containing it will have a name indicating the form of the filetree:

- `config_dump_path/YYYYMMDD-HHMMSS-controller_hostname-config-flattened/` is the flattened (now the default) form, produced by invoking the playbook as follows:
  * `ansible-playbook -i inventory.yml --ask-vault-pass -e config_dump_path=~/ -e export_filetree_flatten=true export-filetree.yml`

  Because `true` is the default value for `export_filetree_flatten`, you could also invoke it like this:
  * `ansible-playbook -i inventory.yml --ask-vault-pass -e config_dump_path=~/ export-filetree.yml`
  

- `config_dump_path/YYYYMMDD-HHMMSS-controller_hostname-config/` is the unflattened form, produced by invoking the playbook as follows:
  * `ansible-playbook -i inventory.yml --ask-vault-pass -e config_dump_path=~/ -e export_filetree_flatten=false export-filetree.yml`
