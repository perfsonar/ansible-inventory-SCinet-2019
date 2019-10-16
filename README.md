# ansible-inventory-SCinet-2019
Ansible perfSONAR Deployment for Supercomputing 2019

Requirements:
 - Ansible bastion host
 - Access to perfSonar infrastructure with base OS and root credentials

How to use this repository:
1.  log on to an Ansible bastion host

```
ssh ansible-bastion-host
```

2.  Get the official perfSONAR Ansible playbook and bootstrap it

```
git clone https://github.com/perfsonar/ansible-playbook-perfsonar.git
cd ansible-playbook-perfsonar
ansible-galaxy install -r  requirements.yml --ignore-errors
```

3.  add this repository to the perfSONAR Ansible playbook

```
git clone git@github.com:perfsonar/ansible-inventory-SCinet-2019.git
```

4.  Use Ansible ping to verify connectivity to targets.

```
ansible \
  --ask-pass \
  --ask-become-pass  \
  -i ansible-inventory-SCinet-2019/inventory \
  all -m ping
```

5. Provision perfSONAR infrastructure

```
ansible-playbook \
  -i ansible-inventory-SCinet-2019/inventory \
  --ask-pass \
  --ask-become-pass  \
  perfsonar.yml
```

6. Provision local changes to perfSONAR infrastructure

```
ansible-playbook \
  -i ansible-inventory-SCinet-2019/inventory \
  --ask-pass \
  --ask-become-pass  \
  ansible-inventory-SCinet-2019/playbooks/perfsonar.yml
```

---

** Some useful commands to troubleshoot the environment **

Manage PWA users:
```
vi ansible-inventory-SCinet-2019/inventory/group_vars/all/perfsonar/ps_pwa.yml
ansible-playbook \
  -i ansible-inventory-SCinet-2019/inventory \
  --ask-pass --ask-become-pass  \
  --limit 'ps-psconfig-web-admin' --tags 'ps::pwa_users'\
  perfsonar.yml
```

Display PWA users:
```
ansible ps-psconfig-web-admin \
  -i ansible-inventory-SCinet-2019/inventory \
  --ask-pass --ask-become-pass  \
  -a "docker exec -it sca-auth pwa_auth listuser --short"
```

Reset Password for PWA user from the command line:
```
ansible ps-psconfig-web-admin \
  -i ansible-inventory-SCinet-2019/inventory \
  --ask-pass --ask-become-pass  \
  -a "docker exec -it sca-auth pwa_auth setpass \
  --username user --password x"
```

Reset Password for PWA user interactively:
```
ansible-playbook \
  -i ansible-inventory-SCinet-2019/inventory \
  --ask-pass --ask-become-pass  \
  roles/ansible-role-perfsonar-psconfig-web-admin/playbooks/pwa_passwd_reset.yml
```
