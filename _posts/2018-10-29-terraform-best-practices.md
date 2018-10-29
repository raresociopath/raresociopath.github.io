---
layout: post
title: "Terraform/AWS Best practices."
comments: true
description: "Terraform best practices"
keywords: "markdown, typography components, dummy content"
---
Alright, I'm not gonna write about what Terraform is and why it is great because 
the official documentation is doing that perfectly.

Instead, I'm going to leave here a couple of notes about best practices using Ansible with AWS.

So, If you don't know what Ansible is and what it is used for please follow this link first:
1. <a href="https://docs.ansible.com/ansible/latest/index.html">Official Ansible documentation</a>


## My Ansible layout

If you are struggling with finding a good layout that will fit your project
here are a couple of blueprints.

> Keep Ansible layout as simple as possible.

An example that just works perfectly.


```
ansible/
├── inventories
├── playbooks
└── README.md
 ``` 

> Do not use project specific names in naming ansible directories.
We want Ansible scripts to be flexible.
Jenkins and so on.

# What next?

```
├── inventories
│   └── aws
│       ├── ec2.ini
│       ├── ec2.py
│       └── group_vars
├── playbooks
│   ├── files
│   │   ├── ssh_keys
│   │   └── ...
│   ├── roles
│   │   ├── base
│   │   ├── java
│   │   └── ...
│   ├── setup_base.yaml
│   └── setup.yaml
└── README.md
```

# Dynamic inventories

AWS infrastructure is dynamic and so should be your scripts. You should never worry about the change of IP’s that’s why I recommend the use of dynamic inventories and good groupings.

```
inventories/aws/
├── ec2.ini
├── ec2.py
└── group_vars
    ├── all # Project variables
    ├── tag_env_dev  # Dev environment variables
    └── tag_env_prod # Prod environment variables

# Inside tag_env_dev

tag_Env_dev/
├── vars.yaml
└── vault.yaml


```

1. More about dynamic inventories <a href="https://docs.ansible.com/ansible/latest/user_guide/intro_dynamic_inventory.html#example-aws-ec2-external-inventory-script" target="_blank">here.</a>


## Entry point


setup.yml is the entry point of your ansible scripts, 
that's why it should be as clean as possible.
Try to only import plays in here.

```
---
- import_playbook: setup_base.yaml
- import_playbook: setup_redis.yaml
...
```


## Running ansible scripts. Tagging
This command should do everything.
```
ansible-playbook -i inventories/aws playbooks/setup.yaml
ansible-playbook -i inventories/aws playbooks/setup.yaml --tags=base
ansible-playbook -i inventories/aws playbooks/setup.yaml --tags=authorized_keys
```


For a full example follow this link:
<a href="https://github.com/raresociopath/ansible-layout-example" >Full example</a>

<div class="divider"></div>
