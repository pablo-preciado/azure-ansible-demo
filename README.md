# Creating VM's in Azure with Ansible

## Introduction

In this repo we will have the playbooks for creating Virtual Machines and the basic infrastructure needed for creating them in Azure.

This is mean to be used by Redhatters or Red Hat partners having access to RHPD as we will deploy an Azure blank environment and then use the resource group that Red Hat provides to create all the other resources.

## Preparing the environment

You need a RHEL8 or RHEL9 server with ansible-navigator installed. Also you'll have to install the Azure CLI tool by running the following command:

```
curl -L https://aka.ms/InstallAzureCli | bash
```

Then you need to export the values for environment variables with the values related to your Azure environment:

```
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
export AZURE_CLIENT_ID=<your-client-id>
export AZURE_SECRET=<your-secret>
export AZURE_TENANT=<your-tenant>
```

Be aware that AZURE_SECRET is your password, not your Client Secret.

Then clone this repo and change to the directory of this repo. This is very important as you need to be in this directory so ansible-navigator catches the config from the files here. These files configure ansible-navigator so it can pass the ENV variables to the execution environment among other things.

Log into quay.io with your Red Hat employee account (you need to have the employee account to use the Execution Environment):

```
podman login quay.io
```

Once there you can just modify the values of vars.yml and then run ansible-navigator:
```
ansible-navigator run az-infra-create.yml -e @vars.yml
```
Then deploy your virtual machines:
```
ansible-navigator run az-vm-create.yml -e @vars.yml
```

Use the same command but changing the playbook for differen operations such as stopping, starting or deleting your virtual machines. This repo also provides the playbook for deleting all your infra.

## Annex I: Finding the publisher, offer, sku and version of your virtual machine image

For me the easiest way of navigating through this is by using the az cli tool. Please, find detailed information [in Microsoft documentation](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/cli-ps-findimage).

## Annex II: Microsoft official documentation about creating Azure Virtual Machines with Ansible

Microsoft has documented what's needed for creating a virtual machine in Azure by using Ansible. [Take a look at it](https://learn.microsoft.com/en-us/azure/developer/ansible/vm-configure?tabs=ansible) if you want to go deeper.