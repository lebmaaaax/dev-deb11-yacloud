# Dev Debian 11 Image for Yandex Cloud

This project automates the creation of a custom `Debian 11` image for `Yandex Cloud` using `Packer` and `Ansible`.
The image provides a ready-to-use development environment with preinstalled tools such as `Git`, `Docker`, `Python3`, `Vim`, `htop`, and more.
To extend the image, modify the existing devtools role or create a new one.

**Features**

Automated build of a custom Debian 11 image

Installation and configuration of essential development tools

Automatic creation of a dev user with SSH access

Preinstalled Docker and Python environment

Modular configuration using Ansible roles

**Folder Structure**

```
packer/
├── ansible/            
│   ├── ansible.cfg
│   ├── playbooks/
│            └── setup_dev_env.yml
│   ├── roles/
│          └── devtools/
│                ├── tasks/main.yml
│                └── files/id_ed25519.pub
├── image.json
├── variables.auto.pkrvars.json.example
└── key.json.example
```

**Requirements**

`Packer` ≥ 1.9

`Ansible` ≥ 2.15

`Yandex Cloud` account

Service account with editor or compute.admin role

key.json file for authentication

SSH key pair using the id_ed25519 algorithm

**Setup**

Copy example configuration files:

cp packer/variables.auto.pkrvars.json.example packer/variables.auto.pkrvars.json
cp packer/key.json.example packer/key.json


Add your public SSH key (used to access created VMs) to:

`packer/ansible/roles/devtools/files/id_ed25519.pub`


Update real values in variables.auto.pkrvars.json and replace service account data in key.json.

**Build Process**

From the packer/ directory, run:
```bash
packer init .
packer validate image.json
packer build -var-file=variables.auto.pkrvars.json image.json
```

After a successful build, the new image will appear in
Yandex Cloud → Compute Cloud → Images as:

debian-11-(timestamp)

**Usage**

Once the image is deployed, connect to the instance via SSH:
```bash
ssh -i ~/.ssh/id_ed25519 dev@<VM_IP>
```

The dev user is created automatically during image provisioning.
Make sure your public key is placed under devtools/files/id_ed25519.pub.

**Installed Packages**

Base Tools	curl, vim, unzip, git, htop, net-tools, jq
Python	python3, python3-pip, virtualenv, ansible, requests
Docker	docker.io
System	sudo with NOPASSWD for dev user

**Notes**

This repository serves as a foundation for building reproducible development environments in Yandex Cloud.
You can easily extend it with additional Ansible roles, integrate Terraform for infrastructure provisioning, or include CI/CD automation for image deployment.

**License**
MIT License

MIT License
