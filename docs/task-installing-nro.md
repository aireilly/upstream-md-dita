# Installing the NUMA Resources Operator using the CLI

As a cluster administrator, you can install the Operator using the CLI.

Prerequisites:

* Install the OpenShift CLI (`oc`).
* Log in as a user with `cluster-admin` privileges.

Once you have completed the prerequisites, complete the following steps:

1. Save the following YAML in the `nro-namespace.yaml` file and create the `Namespace` CR:

      ```yaml
      apiVersion: v1
      kind: Namespace
      metadata:
        name: openshift-numaresources
      ```
   
      ```bash
      $ oc create -f nro-namespace.yaml
      ```

2. Save the following YAML in the `nro-operatorgroup.yaml` file and create the `OperatorGroup` CR:

      ```yaml
      apiVersion: operators.coreos.com/v1
      kind: OperatorGroup
      metadata:
        name: numaresources-operator
        namespace: openshift-numaresources
      spec:
        targetNamespaces:
        - openshift-numaresources
      ```
      
      ```bash
      oc create -f nro-operatorgroup.yaml
      ```

3. Save the following YAML in the `nro-sub.yaml` file and create the subscription for the NUMA Resources Operator:

      ```yaml
      apiVersion: operators.coreos.com/v1alpha1
      kind: Subscription
      metadata:
        name: numaresources-operator
        namespace: openshift-numaresources
      spec:
        channel: "4.19"
        name: numaresources-operator
        source: redhat-operators
        sourceNamespace: openshift-marketplace
      ```
   
      ```bash
      oc create -f nro-sub.yaml
      ```

Verify that the installation succeeded by inspecting the CSV resource in the `openshift-numaresources` namespace. Run the following command:

```bash
oc get csv -n openshift-numaresources
```

```bash
NAME                             DISPLAY                  VERSION   REPLACES   PHASE
numaresources-operator.v4.19.2   numaresources-operator   4.19.2               Succeeded
```
