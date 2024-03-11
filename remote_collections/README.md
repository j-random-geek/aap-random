# Remote Collections in Private Automation Hub

For illustrative purposes only, this directory contains a quick-and-dirty hack to configure remote container registries and Galaxy-style repositories in Private Automation Hub. This works with AAP 2.4 and depends on the `infra.ah_configuration` collection version 2.0.2 or later.

Please see `group_vars/automationhub/ah_config.yml` for some example registries, images, and collection repositories. Sensitive items like passwords and your console.redhat.com Automation Hub token are variables; put those in vaulted form in a variable file or in a (yaml-format) inventory.

There's quite a bit of `no_log: true` set in these playbooks. Without it, sensitive information (like your token for the Red Hat Automation Hub) can appear in your logs etc. Not great.

## Dependencies
This works with Ansible Automation Platform 2.4 and depends on:
- `infra.ah_configuration` collection, version 2.0.2 or later.

## Variables

`create_ah_remotes.yml` has a variable to control use of `no_log`:

| Variable | Type | Required | Default | Comment |
|:---|:---:|:---:|:---:|:---:|
| `create_ah_remotes_secure_logging` | Boolean | N | `true` | Setting this to `false` will allow debugging, but will also show sensitive information like passwords and tokens. Use with caution. |

`sync_ah_remotes.yml` has no similar variable because the input variables are processed in such a way that only the necessary data, which is not sensitive, is passed into each play.


The `group_vars/automationhub/` directory contains a sample configuration of the remote objects to create (for Galaxy content) and sync (for Galaxy content and container registries) contained in the variables `ah_remote_registries`, `ah_remote_images` (for Execution Environment images from remote sources), and `ah_remotes` (for collections from remote sources).

You will need to provide a few things for this to work; most of these are probably best contained, in vaulted form, in your inventory:
| Variable | Type | Required | Default | Comment |
|:---|:---:|:---:|:---:|:---:|
| `ah_admin_user` | String | Y | - | The username of the Private Automation Hub admin user |
| `ah_admin_password` | String| Y | - | The password for the above Private Automation Hub admin user |
| `validate_certs` | Boolean | N  | true | Whether or not to validate SSL certs |
| `rh_cloud_ah_token` | String | Y | - | Your API Token from https://console.redhat.com/ansible/automation-hub/token |
| `pah_port` | Number | N | - | Used to set an alternative port for Private Automation Hub - this will be useful for the containerized version of AAP, for example, which uses port 8444 by default for Private Automation Hub. If not set, the port is omitted. |

Plus any EE registry credentials you may need. Check those group vars mentioned above for examples.

## Playbooks

There are two playbooks here:

### `create_ah_remotes.yml`
This playbook creates remotes and repositories for the Galaxy content defined in the variable `ah_remotes`, as shown in the example [`group_vars/automationhub/ah_config.yml`](group_vars/automationhub/ah_config.yml). Only the basic variables are illustrated; you may also need to add proxies, etc for your setup. Possibly of interest, and shown in the Community collection remote, is the use of the `requirements` keyword to define which collections to sync.

### `sync_ah_remotes.yml`
This playbook triggers a sync of the remote objects (container registries defined in the variable `ah_remote_registries`, container images defined in the variable `ah_remote_images`, and Galaxy repositories defind in `ah_remotes`) - again, as illustrated in `group_vars/automationhub/ah_config.yml`. I personally use this playbook as a scheduled job in AAP 2.4 to update my collection repositories regularly.
