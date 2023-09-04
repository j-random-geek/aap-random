# Remote Collections in Private Automation Hub

For illustrative purposes only, this directory contains a quick-and-dirty hack to configure remote Galaxy-style repositories in Private Automation Hub. This works with AAP 2.4 and depends on the `infra.ah_configuration` collection version 2.02 or later.

## Dependencies
This works with Ansible Automation Platform 2.4 and depends on:
- `infra.ah_configuration` collection, version 2.02 or later.

## Variables

The `group_vars/automationhub/` directory contains a sample configuration of the remote objects to create (for Galaxy content) and sync (for Galaxy content and container registries) contained in the variables `ah_remote_registries` and `ah_remotes`. If you don't define `ah_remote_registries` then you'll probably want to comment out the first task in `sync_ah_remotes.yml` (or maybe I should just split that into a separate playbook).

You will need to provide a few things for this to work; most of these are probably best contained, in vaulted form, in your inventory:
| Variable | Type | Required | Default | Comment |
|:---|:---:|:---:|:---:|:---:|
| `ah_admin_user` | String | Y | - | The username of the Private Automation Hub admin user |
| `ah_admin_password` | String| Y |The password for the above Private Automation Hub admin user |
| `validate_certs` | Boolean | N  | true | Whether or not to validate SSL certs |
| `rh_cloud_ah_token` | String | Y | - | Your API Token from https://console.redhat.com/ansible/automation-hub/token |

## Playbooks

There are two playbooks here:

### `create_ah_remotes.yml`
This playbook creates remotes and repositories for the Galaxy content defined in the variable `ah_remotes`, as shown in the demo `group_vars/automationhub/ah_config.yml`. Only the basic variables are illustrated; you may also need to add proxies, etc for your setup. Possibly of interest, and shown in the Community collection remote, is the use of the `requirements` keyword to define which collections to sync.

### `sync_ah_remotes.yml`
This playbook triggers a sync of the remote objects (container registries defined in the variable `ah_remote_registries` and Galaxy repositories defind in `ah_remotes`) - again, as illustrated in `group_vars/automationhub/ah_config.yml`. I have personally used this playbook as a scheduled job in AAP 2.4 to update my collection repositories every week or so.
