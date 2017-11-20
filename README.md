Post provision steps from attic/contribs
========================================

Static inventory and DNS setup are fallen behind in the attic repo.
Install them as galaxy deps and run them manually from now on:
```
$ git clone https://github.com/bogdando/ocp-inventory inventory
$ git clone https://github.com/openshift/openshift-ansible

$ cp inventory/ansible.cfg .
$ ansible-galaxy install --force -r inventory/galaxy-requirements.yaml \
  -p galaxy

$ source <openstack-creds>

$ ansible-playbook \
  -v openshift-ansible/playbooks/openstack/openshift-cluster/provision.yml \
  -e@inventory/custom.yml

$ ansible-playbook -v inventory/playbooks/static_inventory.yml \
  -e@inventory/custom.yml

$ ansible-playbook -v inventory/playbooks/dns_setup.yml -e@inventory/custom.yml

$ ansible-playbook --become openshift-ansible/playbooks/byo/config.yml \
  -e@inventory/custom.yml
```

