# Core Operator mainfest

Installing Core operator in user's cluster means installing following Kuberenetes resources:

1. Namespace 
2. Pipeline Custom Resource Definition - effectively extending the Kube API with set endpoints tasked to handle Pipeline resources
3. RBAC - For Core operator to be functioning manifest will grant access to several Cluster level objects
Resources to which access is granted:
* **Services, Pods, ConfigMaps, Namespaces and Secrets** on which following operatons can be executed create, delete, get, list, patch, update, watch  
* **Deployments, DaemonSets** on which following operatons can be executed create, delete, get, list, patch, update, watch  
* **Pipelines** on which following operatons can be executed on which following operatons can be executed
4. Service which exposes controller manager instance at port 8443 internally
5. Deployment which is running actually operator container

# Core instance manifest

Installing Core Instance in user's cluster will a single deployment that is running two daemon containers:

1. FromCloud which is tasked with syncing Pipelines that are created/delete in the Cloud API and creating/deleting them in the host cluster
2. ToCloud whichi is tasked with syncing Pipeline statuses back to Cloud API 
