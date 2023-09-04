# Random small AAP things

This will be a place to gather all the little AAP things that I've found useful.

First cab off the rank is [rh-ah-token-refresh](rh-ah-token-refresh), which automates the renewal of your Red Hat Automation Hub token at sso.redhat.com.

Next up, [remote_collections](remote_collections) is an illustration of one possible way to create and sync your collection repositories within Private Automation Hub (NOTE: requires AAP 2.4 and `infra.ah_configuration>=2.02`. It will also sync Execution Environment registries but doesn't yet create them. That's still a work in progress.

When I have the time to bring them in here, I'll add some things to export the AAP configuration in two forms: a single yaml file, and a filetree (easier for humans to parse but occasionally not quite correct). Others will come up over time.
