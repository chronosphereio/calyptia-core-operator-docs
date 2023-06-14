
## Components

Calyptia Core consists from two components:

1. Core operator 

Core operator extends Kubernets cluster with Pipeline Custom Resource Definition and handles its lifecycle. Pipeline is an abstraction which contains all necessary information to run a Fluentbit instance 

2. Core instance

Core instance is a set of daemons running in your cluster which have several functions: 
* Registers itself with Cloud API 
* Fetches Pipelines created in the UI and syncs them with the cluster. Effectively creating or deleting Pipeline resources in the cluster
* Updates Pipelines status in the Cloud API 
