# Random small AAP things

This will be a place to gather all the little AAP things that I've found useful.

- [`rh-ah-token-refresh`](rh-ah-token-refresh) demonstrates automation of the renewal of your Red Hat Automation Hub Galaxy API token. I use this as a scheduled job in AAP, though it's made redundant by the [`remote_collections/sync_ah_remotes.yml`](remote_collections) below.
- [`remote_collections`](remote_collections) is an illustration of one possible way to create and sync your collection repositories and remote execution environment registries and EE images (from those remote registries) within Private Automation Hub.
- [`config_export`](config_export) contains a couple of playbooks to export the Automation Controller configuration in three different forms: one as a single YAML file, and the others as filetree (containing multiple YAML files). They may be illustrative for your configuration-as-code journey.

Where necessary, each of those contains a `collections/requirements.yml` showing the necessary collections. The README in each directory also contains information on dependencies, requirements, and usage.
