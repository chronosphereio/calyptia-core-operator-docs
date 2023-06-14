# Core Operator mainfest

Installing Core operator in user's cluster means installing following Kuberenetes resources:

1. Namespace 
2. Pipeline Custom Resource Definition - effectively extending the Kube API with set endpoints tasked to handle Pipeline resources
3. RBAC - For Core operator to be functioning manifest will grant access to several Cluster level objects
Resources to which access is granted:

* **ServiceAccount**: It defines a service account named `controller-manager` in the `calyptia-core` namespace. Service accounts are used to provide an identity for processes running in pods.

* **Role**: This resource is used to define a role named `leader-election-role` in the `calyptia-core` namespace. The role specifies permissions to perform various operations on resources like configmaps, leases, and events.

ClusterRole: It defines a cluster-level role named manager-role. Cluster roles provide permissions across the entire cluster, and this role has permissions to create, update, and delete resources such as services, pods, deployments, and custom resources of type pipelines in the core.calyptia.com API group.

ClusterRole: This resource defines a cluster role named metrics-reader. It grants read access to the /metrics endpoint, typically used for monitoring and metrics collection.

ClusterRole: Here, a cluster role named proxy-role is defined. It provides permissions to create tokenreviews in the authentication.k8s.io API group and subjectaccessreviews in the authorization.k8s.io API group.

RoleBinding: This resource binds the leader-election-role role to the controller-manager service account in the calyptia-core namespace. It grants the specified role's permissions to the service account.

ClusterRoleBinding: It binds the manager-role cluster role to the controller-manager service account in the calyptia-core namespace. This allows the service account to assume the permissions of the cluster role.

ClusterRoleBinding: This resource binds the proxy-role cluster role to the controller-manager service account in the calyptia-core namespace. It enables the service account to assume the permissions of the cluster role.

These resources collectively define the RBAC configuration for a Kubernetes cluster, granting specific permissions to the controller-manager service account in the calyptia-core namespace.
* **Services, Pods, ConfigMaps, Namespaces and Secrets** on which following operatons can be executed create, delete, get, list, patch, update, watch  
* **Deployments, DaemonSets** on which following operatons can be executed create, delete, get, list, patch, update, watch  
* **Pipelines** on which following operatons can be executed on which following operatons can be executed
4. Service which exposes controller manager instance at port 8443 internally
5. Deployment which is running actually operator container

# Core instance manifest

Installing Core Instance in user's cluster will a single deployment that is running two daemon containers:

1. FromCloud which is tasked with syncing Pipelines that are created/delete in the Cloud API and creating/deleting them in the host cluster
2. ToCloud which is tasked with syncing Pipeline statuses back to Cloud API 

## Resources
Here's a breakdown of the YAML:

* apiVersion and kind specify the version and type of the Kubernetes resource being defined (CustomResourceDefinition in this case).
* metadata contains metadata about the CRD, including annotations and the name.
* spec defines the specification for the CRD, including the API group, resource names, scope, and versions.
* versions specifies the version(s) supported by the CRD. In this case, there is one version defined: v1.
* additionalPrinterColumns defines additional columns to display when printing the resource, including the column name, description, JSON path, and type.
* schema contains the OpenAPI v3 schema definition for the Pipeline resource.
The schema defines various properties and their types, including apiVersion, kind, metadata, spec, and status.
* Within the spec property, there are several nested properties such as cloudResourceID, fluentbit, image, ports, replicasCount, and resources. Each property has its own description and type.
* The status property defines the observed state of the Pipeline resource and includes properties such as daemonsetstatus, deploymentstatus, and servicestatuses.
* Each nested property within status has its own description and type, including sub-properties like conditions, observedGeneration, and ingress.
Overall, this YAML defines the structure and validation rules for the Pipeline resource, including its desired state and observed status.