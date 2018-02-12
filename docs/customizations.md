## Customizations

### Inventory

Our inventory is in `my-inventory` folder. Respective variables are set in `group_vars` folder. There is a `group_vars/all/vault.yml` file consisting of sensitive information. Please follow the comments in these inventory config files to make changes if/when necessary.

Right now we are creating 2 clusters via Kubespray. Hence, we have 2 hosts configuration file namely - `inventory-primary.cfg` and `inventory-cluster2.cfg`. Playbook execution for the respective clusters would require some minor changes in the variable files as mentioned in the _Playbook execution_ section.

*All user specfic sensitive data in the vault file need to be filled before running these playbooks*

Use `ansible-vault edit my-inventory/group_vars/all/vault.yml` to edit vault file.


### Proxy Propagation

To run these Ansible playbooks without any errors we have to make sure that proxy information are propagated for most of the roles which require some kind of network access. Hence, we had to make edits to some of the tasks in various roles to include the `environment: "{{proxy_env}}"` line for the proxy information to be available. Please check the git commits to find those exact changes.

### Kubernetes Apps

These apps are deployed during cluster creation.

#### Rook Operator

Rook operator is installed via Helm in it's own namespace `rook-system`. This operator will control Rook Cluster which is setup as a separate Ansible playbook. All files related to Rook operator is located in `roles/kubernetes-apps/rook/` and a separate tag called `rook` is also created for individual execution of the same. For more information on Rook Controller, checkout ![it's docs](https://rook.github.io/docs/rook/master/kubernetes.html).

### Playbook execution

As we create more than one cluster using the same `my-inventory` folder, we need to ensure that right inventory configuration is used in the below command. E.g. to setup primary cluster
```
$ ansible-playbook -i my-inventory/inventory-primary.cfg cluster.yml -kK -v --ask-vault-pass
```

We also need to make sure that relevant variables are modified before running the above commands. Some important ones to consider:

- entries in the `group_vars/all/vault.yml`
- `inventory_dir`, configurations for External load balancer and `http_proxy` / `https_proxy` in `group_vars/all/vars.yml`
- `weave_seed`, `weave_peers`, `kube_service_addresses`, `kube_pods_subnet` in `group_vars/k8s-cluster.yml`
