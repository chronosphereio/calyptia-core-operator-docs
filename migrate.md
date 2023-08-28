# Migrate from legacy core to core-operator

## Pre-flight

Fetch core instance name from the [UI](https://core.calyptia.com/core-instances)

Install core-operator:

```shell
helm install core-operator calyptia/core-operator
```

## Migrate

On the existing legacy core setup run following commands:

```shell
core_name=<core-instance>
secret_name=="calyptia-$core_name-default-secret"

# Alter existing k8s secret
kubectl get secret $secret_name -o yaml | yq 'del(.metadata.creationTimestamp)'  | yq 'del(.metadata.uid)' | yq 'del(.metadata.resourceVersion)' > secret.yaml
sed '0,/'$name'/{s//'private-key'/}'  secret.yaml | kubectl apply -f -

helm install core-instance calyptia/core-instance --set cloudToken=$TOKEN --set coreInstance=$core_name


## remove leftovers 
kubectl delete deploy calyptia-$core_name-default-deployment
kubectl delete sa calyptia-$core_name-default-service-account
kubectl delete clusterrole calyptia-$core_name-default-cluster-role

instance_id=$(calyptia get instances -o yaml | yq .[0].id)

kubectl delete deploy -l calyptia_aggregator_id=$instance_id
kubectl delete svc -l calyptia_aggregator_id=$instance_id

```

## Migrating services (optional)

Assigning existing service to a pipeline:

Example:

Existing service **my-service**

```yaml
apiVersion: v1
kind: Service
metadata:
name: my-service
namespace: default
spec:
   ports:
   - port: 9880
     protocol: TCP
     targetPort: 9880
type: LoadBalancer
```

To expose pipeline named **my-pipeline** following needs to be done:

```yaml
apiVersion: v1
kind: Service
metadata:
name: my-service
namespace: default
spec:
   ports:
   - port: 9880
     protocol: TCP
     targetPort: 9880
### add selector ####
   selector:
      app.kubernetes.io/component: calyptia-core
      core-pipeline: "my-pipeline"
### end ####
   type: LoadBalancer
```

By doing this pipeline will be exposed on port 9880 of the cluster.
