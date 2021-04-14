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
$ oc projects | grep workshop
  * workshop-user1 - Workshops USER1
    workshop-user2 - Workshops USER2
    workshop-user3 - Workshops USER3
    workshop-user4 - Workshops USER4
    workshop-user5 - Workshops USER5
    workshop-user6 - Workshops USER6
    workshop-user7 - Workshops USER7
Using project "workshop-user1" on server "https://api.docp4.lab.bcnconsulting.com:6443".

$ oc get all
NAME                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
service/gogs                ClusterIP   172.255.20.179   <none>        3000/TCP   39m
service/hostname-postgres   ClusterIP   172.255.237.60   <none>        5432/TCP   39m

NAME                             READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/pods-name-gogs   0/0     0            0           39m

NAME                                        DESIRED   CURRENT   READY   AGE
replicaset.apps/pods-name-gogs-67dd4b956c   0         0         0       39m

NAME                                  READY   AGE
statefulset.apps/pods-name-postgres   0/0     39m
```
