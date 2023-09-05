# Random small AAP things

This will be a place to gather all the little AAP things that I've found useful.

- [`rh-ah-token-refresh`](rh-ah-token-refresh) demonstrats automation of the renewal of your Red Hat Automation Hub Galaxy API token.
- [`remote_collections`](remote_collections) is an illustration of one possible way to create and sync your collection repositories within Private Automation Hub. It will also sync Execution Environment registries but doesn't yet create them. That's still a work in progress.
- [`config_export`](config_export) contains a couple of playbooks to export the Automation Controller configuration in two different forms: one as a single YAML file, and the other as a filetree (containing multiple YAML files). They may be illustrative for your configuration-as-code journey.

Where necessary, each of those contains a collections/requirements.yml showing the necessary collections. The README in each directory also contains information on dependency requirements, both collections and Python.
