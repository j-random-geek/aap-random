# Refreshing your Red Hat Automation Hub API Token

For the cloud offering currently at https://console.redhat.com/ansible/automation-hub

This is a quick-and-dirty hack and I'm sure there are a million others out there. The idea is to create a JT using this playbook, and schedule the JT every week or so to keep the token valid.

## Variables

Only one variable needs to be defined:
- `rh_cloud_ah_token`: The token you're refreshing. **Mandatory**.

That variable really ought to be vaulted. Just saying.

