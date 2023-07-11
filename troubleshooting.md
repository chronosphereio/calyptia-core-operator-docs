# Troubleshooting

## Operator

In case there is an issue with running core-operator check following components to get more details:

```shell
kubectl logs -n core-operator deployment/controller-manager
```

The command will return logs of the operator pod.

An example log of successfully funning operator:

```shell
{"level":"info","ts":"2023-07-06T10:49:41Z","logger":"controller-runtime.metrics","msg":"Metrics server is starting to listen","addr":":8080"}
{"level":"info","ts":"2023-07-06T10:49:41Z","logger":"setup","msg":"Successfully started","controller":"Deployment"}
{"level":"info","ts":"2023-07-06T10:49:41Z","logger":"setup","msg":"Successfully started","controller":"DaemonSet"}
{"level":"info","ts":"2023-07-06T10:49:41Z","logger":"setup","msg":"Successfully started","controller":"Secret"}
{"level":"info","ts":"2023-07-06T10:49:41Z","logger":"setup","msg":"starting manager"}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting server","path":"/metrics","kind":"metrics","addr":"[::]:8080"}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting server","kind":"health probe","addr":"[::]:8081"}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting EventSource","controller":"daemonset","controllerGroup":"apps","controllerKind":"DaemonSet","source":"kind source: *v1.DaemonSet"}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting EventSource","controller":"pipeline","controllerGroup":"core.calyptia.com","controllerKind":"Pipeline","source":"kind source: *v1.Pipeline"}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting Controller","controller":"daemonset","controllerGroup":"apps","controllerKind":"DaemonSet"}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting EventSource","controller":"pipeline","controllerGroup":"core.calyptia.com","controllerKind":"Pipeline","source":"kind source: *v1.Deployment"}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting EventSource","controller":"pipeline","controllerGroup":"core.calyptia.com","controllerKind":"Pipeline","source":"kind source: *v1.DaemonSet"}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting EventSource","controller":"pipeline","controllerGroup":"core.calyptia.com","controllerKind":"Pipeline","source":"kind source: *v1.ConfigMap"}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting Controller","controller":"pipeline","controllerGroup":"core.calyptia.com","controllerKind":"Pipeline"}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting EventSource","controller":"deployment","controllerGroup":"apps","controllerKind":"Deployment","source":"kind source: *v1.Deployment"}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting Controller","controller":"deployment","controllerGroup":"apps","controllerKind":"Deployment"}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting EventSource","controller":"secret","controllerGroup":"","controllerKind":"Secret","source":"kind source: *v1.Secret"}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting Controller","controller":"secret","controllerGroup":"","controllerKind":"Secret"}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting workers","controller":"daemonset","controllerGroup":"apps","controllerKind":"DaemonSet","worker count":1}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting workers","controller":"secret","controllerGroup":"","controllerKind":"Secret","worker count":1}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting workers","controller":"deployment","controllerGroup":"apps","controllerKind":"Deployment","worker count":1}
{"level":"info","ts":"2023-07-06T10:49:41Z","msg":"Starting workers","controller":"pipeline","controllerGroup":"core.calyptia.com","controllerKind":"Pipeline","worker count":1}
```

## Troubleshooting a Pipeline

Listing pipelines:

```shell
kubectl get pipeline
NAME          STATUS
with-secret   STARTED
```

There a few things that can be done:

1. Check operator log to see if there is something specific wrong with the pipeline do step 1 from above.
2. Check pipeline status details:

```shell
kubectl describe pipeline with-secret
...
<truncated>
...
Status:
  Deploymentstatus:
    Available Replicas:  1
    Conditions:
      Last Transition Time:  2023-07-10T10:24:44Z
      Last Update Time:      2023-07-10T10:24:44Z
      Message:               Deployment has minimum availability.
      Reason:                MinimumReplicasAvailable
      Status:                True
      Type:                  Available
      Last Transition Time:  2023-07-10T10:24:36Z
      Last Update Time:      2023-07-10T10:24:44Z
      Message:               ReplicaSet "with-secret-657b8c6c54" has successfully progressed.
      Reason:                NewReplicaSetAvailable
      Status:                True
      Type:                  Progressing
    Observed Generation:     5
    Ready Replicas:          1
    Replicas:                1
    Updated Replicas:        1
  Status:                    STARTED
```

For details about pipeline status please go to [CRD Reference](./docs/crd-reference#pipelinestatus)

If this doesn't help let's go one step further
Because in this case a pipeline is of *Deployment* kind we can look into pipeline logs:

```shell
kubectl logs deploy/with-secret
```

The result should ressemble something like this:

```shell
Found 3 pods, using pod/with-secret-6567fbd648-wrxgn
Calyptia Core v23.6, patch_level=1
* https://calyptia.com

[2023/07/10 10:34:16] [ info] [calyptia core] version=23.6, patch=1, pid=1
[2023/07/10 10:34:16] [ info] [storage] ver=1.4.0, type=memory, sync=normal, checksum=off, max_chunks_up=128
[2023/07/10 10:34:16] [ info] [cmetrics] version=0.6.1
[2023/07/10 10:34:16] [ info] [ctraces ] version=0.3.0
[2023/07/10 10:34:16] [ info] [input:dummy:dummy.0] initializing
[2023/07/10 10:34:16] [ info] [input:dummy:dummy.0] storage_strategy='memory' (memory only)
[2023/07/10 10:34:16] [ info] [output:nrlogs:nrlogs.0] configured, hostname=log-api.newrelic.com:443
[2023/07/10 10:34:16] [ info] [sp] stream processor started
[2023/07/10 10:34:17] [error] [tls] error: unexpected EOF
[2023/07/10 10:34:17] [error] [output:nrlogs:nrlogs.0] no upstream connections available
[2023/07/10 10:34:17] [ warn] [engine] failed to flush chunk '1-1688985256.913432285.flb', retry in 7 seconds: task_id=0, input=dummy.0 > output=nrlogs.0 (out_id=0)
```

This will give you Fluentbit logs. To troubleshoot fluentbit please refer to <https://docs.fluentbit.io/manual/administration/troubleshooting>

### Failed pipeline

If listing of pipelines shows `FAILED` status:

```shell
# kubectl get pipeline
NAME      STATUS
parsers   FAILED
```

This is usually indication that the underlying containers are failing. To check what is happening first check the replica set of pipeline.

```shell
kubectl describe pipeline parsers
```

To determine which kind of pipeline look under status property. In this case it is of the deployment kind.

```shell
Status:
  Deploymentstatus:
    Conditions:
      Last Transition Time:  2023-07-10T12:17:08Z
      Last Update Time:      2023-07-10T12:18:30Z
      Message:               ReplicaSet "parsers-688bc6fcff" has successfully progressed.
      Reason:                NewReplicaSetAvailable
      Status:                True
      Type:                  Progressing
      Last Transition Time:  2023-07-10T12:18:31Z
      Last Update Time:      2023-07-10T12:18:31Z
      Message:               Deployment does not have minimum availability.
      Reason:                MinimumReplicasUnavailable
      Status:                False
      Type:                  Available
    Observed Generation:     10
    Replicas:                1
    Unavailable Replicas:    1
    Updated Replicas:        1
  Status:                    FAILED
```

Checkout the deployment status:

```shell
kubectl get deployment parsers
```

```shell
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
parsers   0/1     1            0           5m37s
```

Next look into deployment logs:

```shell
# kubectl logs deployment/parsers
```

The result:

```shell
Calyptia Core v23.6, patch_level=1
* https://calyptia.com

[2023/07/10 12:21:20] [ info] [calyptia core] version=23.6, patch=1, pid=1
[2023/07/10 12:21:20] [ info] [storage] ver=1.4.0, type=memory, sync=normal, checksum=off, max_chunks_up=128
[2023/07/10 12:21:20] [ info] [cmetrics] version=0.6.1
[2023/07/10 12:21:20] [ info] [ctraces ] version=0.3.0
[2023/07/10 12:21:20] [ info] [input:tail:tail.0] initializing
[2023/07/10 12:21:20] [ info] [input:tail:tail.0] storage_strategy='memory' (memory only)
[2023/07/10 12:21:20] [error] [multiline] parser 'cri12' not registered
[2023/07/10 12:21:20] [error] [input:tail:tail.0] could not load multiline parsers
[2023/07/10 12:21:20] [error] failed initialize input tail.0
[2023/07/10 12:21:20] [error] [engine] input initialization failed
[2023/07/10 12:21:20] [error] [lib] backend failed
```

In this example the parser of type cri12 cannot be found.

### No status pipeline

If the created pipeline doesn't show any status:

```shell
# kubectl get pipeline
NAME            STATUS
in-http
```

The resource will not have any status fields. So the next step to start looking into is the operator log.

```shell
 kubectl logs -n core-operator deployment/controller-manager
```

Resulting output should contain something similar to this:

```shell
{"level":"error","ts":"2023-07-10T12:13:22Z","msg":"Reconciler error","controller":"pipeline","controllerGroup":"core.calyptia.com","controllerKind":"Pipeline","Pipeline":{"name":"in-http","namespace":"default"},"namespace":"default","name":"in-http","reconcileID":"68b274c8-3cd2-49fd-860e-71c80b1e96f7","error":"yaml: unmarshal errors:\n  line 1: cannot unmarshal !!seq into fluentbitconfig.Config","stacktrace":"sigs.k8s.io/controller-runtime/pkg/internal/controller.(*Controller).reconcileHandler\n\t/go/pkg/mod/sigs.k8s.io/controller-runtime@v0.14.1/pkg/internal/controller/controller.go:329\nsigs.k8s.io/controller-runtime/pkg/internal/controller.(*Controller).processNextWorkItem\n\t/go/pkg/mod/sigs.k8s.io/controller-runtime@v0.14.1/pkg/internal/controller/controller.go:274\nsigs.k8s.io/controller-runtime/pkg/internal/controller.(*Controller).Start.func2.2\n\t/go/pkg/mod/sigs.k8s.io/controller-runtime@v0.14.1/pkg/internal/controller/controller.go:235"}
```

This indicates that the Pipeline `spec.fluentbit.config` property has not been provided in the correct YAML format which is blocking further reconciliation of the pipeline.

## core-instance (sync)

Sync or core-instance is an adapter that synchronizes pipeline resources between Cloud API and the cluster and vice versa.

Core instance is a single deployment with two containers:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sync
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app.kubernetes.io/name: sync
      app.kubernetes.io/instance: calyptia-core
  template:
    metadata:
      labels:
        app.kubernetes.io/name: sync
        app.kubernetes.io/instance: calyptia-core
    spec:
      containers:
      - env:
        - name: CORE_INSTANCE
          value: test-client
        - name: NAMESPACE
          value: core-operator
        - name: CLOUD_URL
          value: https://cloud-api.calyptia.com
        - name: TOKEN
          value: eyJUb2tlbklEIjoiMmVkZTc1NDktZmE2OS00N2ZiLWI5MTQtMDQ0M2NhMDVlYmUyIiwiUHJvamVjdElEIjoiYzY5NzkzYTQtNmFiYy00NGI3LWJjODctZDc1NjgxODNhYTY1In0.xMGQYDqK1bojmal74y7oMh8O8wsesh8BHPdWL-hn-LCIEZbtbBns3DvzWW4B7xEH
        - name: INTERVAL
          value: 15s
        - name: CLUSTER_LOGGING
          value: "false"
        - name: NO_TLS_VERIFY
          value: "true"
        image: ghcr.io/calyptia/core-operator/sync-from-cloud:1.0.4
        imagePullPolicy: Always
        name: from-cloud
      - env:
        - name: CORE_INSTANCE
          value: test-client
        - name: NAMESPACE
          value: core-operator
        - name: CLOUD_URL
          value: https://cloud-api.calyptia.com
        - name: TOKEN
          value: your_token
        - name: INTERVAL
          value: 15s
        - name: NO_TLS_VERIFY
          value: "true"
        image: ghcr.io/calyptia/core-operator/sync-to-cloud:1.0.4
        imagePullPolicy: Always
        name: to-cloud
      serviceAccount: controller-manager
```

* to-cloud container will push every newly created/update pipeline to Cloud API as well it will ensure that namespaces from the cluster are synchronized with Cloud API
* from-cloud container will pull all pipelines from CloudAPI and push them to the cluster as Pipeline Custom Resources:

Checking status of core-instance

```shell
kubectl get deploy -n core-operator   core-instance
NAME            READY   UP-TO-DATE   AVAILABLE   AGE
core-instance   0/1     1            0           5m20s
```

Evidently the deployment is failing to start as READY = 0/1
Checking deployment logs:

```shell
kubectl logs   -n core-operator   deploy/core-instance
Defaulted container "from-cloud" out of: from-cloud, to-cloud
error occurred while setting up configuration Get "https://cloud-api.calyptia.com/v1/projects/c69793a4-6abc-44b7-bc87-d7568183aa65/core_instances?name=test-client": remote error: tls: handshake failure
```

Or individually for every container:

From Cloud

```shell
kubectl logs   -n core-operator   deploy/core-instance  from-cloud
```

To Cloud

```shell
kubectl logs   -n core-operator   deploy/core-instance  to-cloud
```
