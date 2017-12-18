## Customizations

### Inventory

Our inventory is in `my-inventory` folder. Respective variables are set in `group_vars` folder. There is a `group_vars/all/vault.yml` file consisting of sensitive information. Please follow the comments in these inventory config files to make changes if/when necessary.

*All user specfic sensitive data in the vault file need to be filled before running these playbooks*

### Proxy Propagation

To run these Ansible playbooks without any errors we have to make sure that proxy information are propagated for most of the roles which require some kind of network access. Hence, we had to make edits to some of the tasks in various roles to include the `environment: "{{proxy_env}}"` line for the proxy information to be available. Please check the git commits to find those exact changes.

### Kubernetes Apps

These apps are deployed during cluster creation.

#### Rook Operator

Rook operator is installed via Helm in it's own namespace `rook-system`. This operator will control Rook Cluster which is setup as a separate Ansible playbook. All files related to Rook operator is located in `roles/kubernetes-apps/rook/` and a separate tag called `rook` is also created for individual execution of the same. For more information on Rook Controller, checkout ![it's docs](https://rook.github.io/docs/rook/master/kubernetes.html).
