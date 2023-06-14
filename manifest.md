# Core Operator mainfest

Installing Core operator in user's cluster means installing following Kuberenetes resources:

1. Namespace 
2. Pipeline Custom Resource Definition - effectively extending the Kube API with set endpoints tasked to handle Pipeline resources
3. RBAC - For Core operator to be functioning manifest will grant access to several Cluster level objects
Resources to which access is granted:

* **ServiceAccount**: It defines a service account named `controller-manager` in the `calyptia-core` namespace. Service accounts are used to provide an identity for processes running in pods.

* **Role**: This resource is used to define a role named `leader-election-role` in the `calyptia-core` namespace. The role specifies permissions to perform various operations on resources like configmaps, leases, and events.

* **ClusterRole**: It defines a cluster-level role named `manager-role`. Cluster roles provide permissions across the entire cluster, and this role has permissions to create, update, and delete resources such as `services`, `pods`, `deployments`, and custom resources of type `pipelines` in the `core.calyptia.com` API group.

* **ClusterRole**: This resource defines a cluster role named `metrics-reader`. It grants read access to the `/metrics` endpoint, typically used for monitoring and metrics collection.

* **ClusterRole**: Here, a cluster role named `proxy-role` is defined. It provides permissions to create `tokenreviews` in the `authentication.k8s.io` API group and `subjectaccessreviews` in the `authorization.k8s.io` API group.


* **ClusterRoleBinding**: It binds the `manager-role` cluster role to the `controller-manager` service account in the `calyptia-core` namespace. This allows the service account to assume the permissions of the cluster role.

* **ClusterRoleBinding**: This resource binds the `proxy-role` cluster role to the `controller-manager` service account in the `calyptia-core` namespace. It enables the service account to assume the permissions of the cluster role.

These resources collectively define the RBAC configuration for a Kubernetes cluster, granting specific permissions to the `controller-manager` service account in the `calyptia-core` namespace.

* **Services, Pods, ConfigMaps, Namespaces and Secrets** on which following operatons can be executed create, delete, get, list, patch, update, watch  
* **Deployments, DaemonSets** on which following operatons can be executed create, delete, get, list, patch, update, watch  
* **Pipelines** on which following operatons can be executed on which following operatons can be executed
* **Service** which exposes controller manager instance at port 8443 internally
* **Deployment** which is running actual operator container


## Resources

* `apiVersion` and kind specify the version and type of the Kubernetes resource being defined (CustomResourceDefinition in this case).
* `metadata` contains metadata about the CRD, including annotations and the name.
* `spec` defines the specification for the CRD, including the API group, resource names, scope, and versions.
* `versions` specifies the version(s) supported by the CRD. In this case, there is one version defined: v1.
* Within the `spec` property, there are several nested properties such as `cloudResourceID`, `fluentbit`, `image`, `ports`, `replicasCount`, and `resources`. Each property has its own description and type.
    * `fluentbit`
        * `config` - which represents fluent-bit configuration in YAML format
        * `parsers` - which represents fluent-bit parser configuration in YAML format
        * `files` - is a list of files in format name and content
        * `secrets` - is a list of secrets in format name and content
    * `cloudResrouceID` - is the UID under which the pipeline is stored in Cloud API
    * `image` - fluent bit image
    * `ports` - ports for the pipeline. Note: with definition of each port additional kubernetes loadbalancer service will be created
    * `replicasCount` - number of replica pod of the pipeline to be created

* The `status` property defines the observed state of the Pipeline resource and includes properties such as `daemonsetstatus`, `deploymentstatus`, and `servicestatuses`.
* Each nested property within status has its own description and type, including sub-properties like conditions, observedGeneration, and ingress.

For details please take a look [CRD Reference](crd-reference.md)


## Core instance manifest

Installing Core Instance in a cluster will create a single deployment that is running two daemon containers:

1. FromCloud which is tasked with syncing Pipelines that are created/delete in the Cloud API and creating/deleting them in the host cluster
2. ToCloud which is tasked with syncing Pipeline statuses back to Cloud API 
