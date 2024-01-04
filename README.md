# Vagrant Ansible Automation Platform Lab

The purpose of this lab is to practice AAP Installation and configuration. The Vagrantfile will provision single cluster of AAP with 3 nodes:

| Function | IP Address |
|----------|------------|
| Hybrid Node | 192.168.56.10 |
| Private Automation Hub | 192.168.56.11 |
| Database | 192.168.56.12 |

## Get RHEL License

This lab using RHEL9 image and utilizing vagrant-registration plugin to register RHEL to RHSM

The following command will install vagrant-registration plugin

```bash
vagrant plugin install vagrant-registration
```

Get RHEL Developer license in https://developers.redhat.com/products/rhel/download 

## Obtain Ansible Automation Platform Installer 

Register for trial and download AAP bundle from the following link:

https://www.redhat.com/en/technologies/management/ansible/trial

Follow the following guidance to download manifest file

https://access.redhat.com/documentation/id-id/red_hat_ansible_automation_platform/2.4/html/red_hat_ansible_automation_platform_operations_guide/assembly-aap-obtain-manifest-files

## Reducing Memory Usage

Since this is aimed for lab environment, you can reduce the memory size and edit the following file in the AAP Installer bundle

```bash
collections/ansible_collections/ansible/automation_platform_installer/roles/preflight/defaults/main.yml
```
Adjust the server memory
```yaml
---
## Operational parameters

required_ram: 7400
ignore_preflight_errors: false

## Configurable parameters
```

## Provision the lab

Export your Red Hat Subscription Management username and password

```bash
export SUB_USERNAME=""
export SUB_PASSWORD=""
vagrant up
```
Start the AAP Installation from the hybrid node

```bash
vagrant ssh hybrid
```


