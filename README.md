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

4. Provision perfSONAR infrastructure.  Optionally use Ansible's 'ping' to
   test connectivity first.

```
ansible-playbook \
  -i ansible-inventory-SCinet-2019/inventory \
  perfsonar.yml
```

5. Provision local changes to perfSONAR infrastructure

```
ansible-playbook \
  -i ansible-inventory-SCinet-2019/inventory \
  ansible-inventory-SCinet-2019/playbooks/perfsonar.yml
```

---

** Some useful commands to troubleshoot the environment **

Use Ansible ping to verify connectivity to targets.

```
ansible \
  -i ansible-inventory-SCinet-2019/inventory \
  all -m ping
```

Display auth interfaces on Archivers:
```
ansible ps-archives \
  -i ansible-inventory-SCinet-2019/inventory \
  -a "/usr/sbin/esmond_manage list_user_ip_address"
```

Delete an auth interface on Archivers:
```
ansible ps-archives \
  -i ansible-inventory-SCinet-2019/inventory \
  -a "/usr/sbin/esmond_manage delete_user_ip_address USERNAME IPADDR"
```
