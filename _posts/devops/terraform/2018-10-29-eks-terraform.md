---
layout: post
title: "Provisioning Autoscalable EKS with Terraform"
comments: true
description: "Ansible architecture"
keywords: "markdown, typography components, dummy content"
category: terraform
---

Read before proceding:
1. <a href="https://docs.ansible.com/ansible/latest/index.html" target="_blank"> What is ansible</a>
2. <a href="https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/" target="_blank"> What is kubernetes </a>
3. <a href="https://github.com/kubernetes/kops/blob/master/README.md" target="_blank">What is Kops </a>

So, we are gonna write ansible scripts to provision k8s with the help of kops.

The code is <a href="https://github.com/raresociopath/k8s-ansible" target="_blank"> here </a>



```
k8s-ansible/
├── ansible.cfg
├── cluster.conf.d
├── create-cluster.yaml
├── delete-cluster.yaml
├── group_vars
├── inventory
├── kops-clusters
├── README.md
├── roles
├── utils
└── validate-cluster.yaml

``` 

## Create cluster

```
---
- include_role:
    name: base

- name: "Create cluster {{ cluster_full_name }}"
  shell: >
    kops create cluster {{ cluster_full_name }}
    --cloud-labels="Team=MyTeam"
    --admin-access=<to be changed>/32
    --kubernetes-version={{ kube_version | quote }}
    --zones={{ aws_zones | quote }}
    --dns-zone={{ dns_zone | default('') | quote }}
    --master-count={{ master_count | quote }}
    --master-size={{ master_size | quote }}
    --master-volume-size={{ master_volume_size | quote }}
    --cloud=aws
    --output yaml
    --state=s3://{{ kops_state_bucket | quote }}
    --node-size={{ node_size | quote }}
    --node-count={{ nodes_count | quote }}
    --node-volume-size={{ node_volume_size | quote }}
    --topology=public
    --encrypt-etcd-storage
    --networking=canal
- name: "Apply extra confs on cluster"
  shell: >
    ./utils/genClusterConfig.py
    --cluster-name={{ cluster_full_name }}
    --cluster-state=s3://{{ kops_state_bucket | quote }}
    --conf-path=cluster.conf.d
    --output=./tmp-{{ cluster_full_name }}.yaml
    &&
    kops replace --name {{ cluster_full_name }} --state=s3://{{ kops_state_bucket | quote }} -f ./tmp-{{ cluster_full_name }}.yaml
    &&
    rm -f ./tmp-{{ cluster_full_name }}.yaml
- name: "Apply extra confs on nodes"
  shell: >
    ./utils/genClusterConfig.py
    --cluster-name={{ cluster_full_name }}
    --cluster-state=s3://{{ kops_state_bucket | quote }}
    --conf-path=nodes.conf.d
    --component=ig
    --output=./tmp-{{ cluster_full_name }}-nodes.yaml
    &&
    kops replace --name {{ cluster_full_name }} --state=s3://{{ kops_state_bucket | quote }} -f ./tmp-{{ cluster_full_name }}-nodes.yaml
    &&
    rm -f ./tmp-{{ cluster_full_name }}-nodes.yaml
- name: "Apply extra confs on master"
  shell: >
    ./utils/genClusterConfig.py
    --cluster-name={{ cluster_full_name }}
    --cluster-state=s3://{{ kops_state_bucket | quote }}
    --conf-path=master.conf.d
    --component=master-{{ item }}
    --output=./tmp-{{ cluster_full_name }}-master-{{ item }}.yaml
    &&
    kops replace --name {{ cluster_full_name }} --state=s3://{{ kops_state_bucket | quote }} -f ./tmp-{{ cluster_full_name }}-master-{{ item }}.yaml
    &&
    rm -f ./tmp-{{ cluster_full_name }}-master-{{ item }}.yaml
  with_items: "{{ aws_zones.split(',') }}"
  loop_control:
    index_var: index
  when: index < master_count

- name: "Apply plan creation for cluster {{ cluster_name }}"
  shell: >
    kops update cluster
    --name {{ cluster_full_name }}
    --state=s3://{{ kops_state_bucket | quote }}
    --yes
```


For a full example follow this link:
<a href="https://github.com/raresociopath/k8s-ansible"  target="_blank">Full example</a>

<div class="divider"></div>
