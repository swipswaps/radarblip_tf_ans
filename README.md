

# Install RadarBlip deployment via Terraform and Ansible:

You will need to input your AWS user credentials (with a user that has privileges to make an (EC2 etc..) instance) into the `/terraform_aws/variables.tf` file.

This folder contains sub folders with /ansible that holds the playbook file you want to deploy on the server, default playbook filename is:  radarblip.yml
You will need to alter the `main.tf` file in the `terraform_aws folder` if you plan to change this filename.

To initialize the Terraform , go into `/terraform_aws`  folder and run `terraform init` -  This will download the needed plugins Terraform uses to create an AWS instance.

Go back to the root directory of this app, containing this README.md file, and execute the install script, or run `ansible-playbook main.yml` from command line.  

This calls a playbook which referes to the `/terraform_aws` directory.
Runs the Terraform `main.tf` 
this script eventually sets up ansible folder to upload the ansible file (radarblip.yml) to the newly created remote server using the `ubuntu` ssh keys.
The remote server then follows the playbook that was uploaded, and installs the node-red instance along with placing a new settings, flows and all the node-red nodes needed to run.

Visit your newly created ip node-red instance:   
https://xxx.xxxx.xxx.xxx:18800/
Must be HTTPS

Default user and login are:  radarblip / radarblip!

*********IMPORTANT NOTICE**************
Please consult the node-red documentary on adding users, and changing passwords for node-red, otherwise your node-red instance is considered insecure, as the login and password are default, and remote exec commands can potentially be executed on that machine.
***************************************


###### BELOW IS MAINTAINED FROM THE ORIGINAL GIT SOURCE EXAMPLE

# Ansible and Terraform - Better Together
This repository contains code examples for running Terraform and Ansible together in different configurations.

## Run Ansible from Terraform
### With `local_exec`
Use the code examples in the `terraform_gcp` or `terraform_azure` folders to see how this is done. Basically there are two steps. First is a remote exec which forces Terraform to wait until SSH is running to run Ansible. This can be anything, even an `echo "Hello World"` command. Then we execute an `ansible-playbook` command from the same machine where we ran `terraform apply`.

### With `remote_exec`
The code example in the `terraform_gcp` directory has code for remote exec commented out. You can comment out the local_exec code and run this to have Ansible run on the remote host. With this method we first copy our playbook to the remote host, then we install and run Ansible locally there.

## Run Terraform from Ansible
Ansible has a Terraform module that can trigger Terraform deployments and plans. You can also pull resource information back into Ansible:
https://docs.ansible.com/ansible/devel/modules/terraform_module.html

## Build System Images with Packer and Ansible
HashiCorp's Packer tool allows you to use your existing Ansible playbooks to easily build machine images on the cloud or virtualization platform of your choice. Packer uses a JSON file for configuration, and is run from the command line. 

## Integrate HashiCorp Vault with Ansible
You can easily fetch secrets from Vault using the Hashi Vault Ansible plugin:
https://docs.ansible.com/ansible/devel/plugins/lookup/hashi_vault.html

This plugin is based on the excellent Python HVAC library:
https://github.com/hvac/hvac
