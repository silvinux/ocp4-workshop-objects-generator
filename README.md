# OCP4 Workshop namespaces and policies generator

Repository hosts manifests to deploy a kubernetes gogs + postgres app. Is based on the Labs Enablement material. It is a working skeleton that learners will use in conjunction with the [enablement-docs](https://github.com/rht-labs/enablement-docs).

### Basic usage

1. Clone this repository.
1. Log on to an OpenShift server `oc login -u <user> https://<server>:<port>/`
1. Install the required [openshift-applier](https://github.com/redhat-cop/openshift-applier) dependency:
```bash
$ ansible-galaxy install -r requirements.yml --roles-path=roles
```

1. Run the play book using this to generate the file variable "../enablement-ci-cd/inventory/host_vars/projects-and-policies.yml", which is the skeleton of the project.
```bash
$ ansible-playbook generator-template-ns.yml --tags create-template -e '{namespace_prefix: workshop}' -e @attendant_list.yml
```

1. Run the play book using this to create namespaces, services, desployment and statefulsets needed to break apart the exercise.
```bash
$ ansible-playbook apply.yml -i inventory/ -e target=bootstrap
```

1. Once is finished, you should've deployed the objects you need.
```bash

```
