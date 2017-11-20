Post provision steps from attic/contribs
========================================

Static inventory and DNS setup are fallen behind in the attic repo.
Install them as galaxy deps and run them manually from now on.

Prepare for deployment
```
$ git clone https://github.com/bogdando/ocp-inventory inventory
$ git clone https://github.com/openshift/openshift-ansible

$ cp inventory/ansible.cfg .
$ ansible-galaxy install --force -r inventory/galaxy-requirements.yaml \
  -p galaxy

$ source <openstack-creds>
```

Provision Nova servers et al
```
$ ansible-playbook \
  -v openshift-ansible/playbooks/openstack/openshift-cluster/provision.yml \
  -e@inventory/custom.yml
```

Provision DNS server to be configured with either CASL (default) or (TBD) OSP 10
ref arch
```
$ ansible-playbook -v inventory/playbooks/deploy-dns.yaml \
  -e@inventory/custom.yml
  -e osp10_ref_arch_dns=<true|false>
```

Generate static inventory and SSH config for bastion access
```
$ ansible-playbook -v inventory/playbooks/static_inventory.yml \
  -e@inventory/custom.yml
```

Configure DNS and populate records (TODO recover Neutron subnet update steps)
```
$ ansible-playbook -v inventory/playbooks/dns_setup.yml -e@inventory/custom.yml
```

Deploy OpenShift
```
$ ansible-playbook --become openshift-ansible/playbooks/byo/config.yml \
  -e@inventory/custom.yml
```
