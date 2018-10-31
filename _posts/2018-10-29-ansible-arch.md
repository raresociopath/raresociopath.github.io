---
layout: post
title: "Ansible/AWS: Best Practices."
comments: true
description: "Ansible architecture"
keywords: "markdown, typography components, dummy content"
---
Alright, I'm not gonna write about what Ansible is and why it is great because 
the official documentation is doing that perfectly.

Instead, I'm going to leave here a couple of notes about best practices using Ansible with AWS.

So, If you don't know what Ansible is and what it is used for please follow this link first:
1. <a href="https://docs.ansible.com/ansible/latest/index.html">Official Ansible documentation</a>


## Ansible Directory layout that always worked for me

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
│       └── group_vars # variables for particular groups
├── playbooks
│   ├── files # files needed by some roles
│   │   ├── ssh_keys
│   │   └── ...
│   ├── roles
│   │   ├── base
│   │   ├── java
│   │   └── ...
│   ├── setup_base.yaml
│   └── setup.yaml # master playbook 
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


## Master playbook ( master ) 


setup.yml is the entry point (master) playbook of your ansible scripts, 
that's why it should be as clean as possible.
Try to only import plays in here.

```
---
- import_playbook: setup_base.yaml
- import_playbook: setup_redis.yaml
...
```

## TAGS
A very important part of the ansible playbooks is tags.
My recomendations is not to use tags in roles.
I use tags only in the high level playbooks and the dynamic inventory tags.

## Running ansible scripts. Tagging
This command should do everything.
```
ansible-playbook -i inventories/aws playbooks/setup.yaml
ansible-playbook -i inventories/aws playbooks/setup.yaml --tags=base
ansible-playbook -i inventories/aws playbooks/setup.yaml --tags=authorized_keys
```




For a full example follow this link:
<a href="https://github.com/raresociopath/ansible-layout-example" target="_blank">Full example</a>

<div class="divider"></div>
