# Installing Calyptia Core

1. Running Kubernetes cluster
2. Kubectl 

## Installation options
### Calyptia CLI 

To install Calyptia CLI follow these [instructions](https://github.com/calyptia/cli#install)

CLI must be at version v1.3.8 or greater

```
Setup a new core operator instance

Usage:
  calyptia install operator [flags]

Aliases:
  operator, opr

Flags:
  -h, --help                                help for operator
      --kube-as string                      Username to impersonate for the operation
      --kube-as-group stringArray           Group to impersonate for the operation, this flag can be repeated to specify multiple groups.
      --kube-as-uid string                  UID to impersonate for the operation
      --kube-certificate-authority string   Path to a cert file for the certificate authority
      --kube-client-certificate string      Path to a client certificate file for TLS
      --kube-client-key string              Path to a client key file for TLS
      --kube-cluster string                 The name of the kubeconfig cluster to use
      --kube-context string                 The name of the kubeconfig context to use
      --kube-disable-compression            If true, opt-out of response compression for all requests to the server
      --kube-insecure-skip-tls-verify       If true, the server's certificate will not be checked for validity. This will make your HTTPS connections insecure
  -n, --kube-namespace string               If present, the namespace scope for this CLI request
      --kube-password string                Password for basic authentication to the API server
      --kube-proxy-url string               If provided, this URL will be used to connect via proxy
      --kube-request-timeout string         The length of time to wait before giving up on a single server request. Non-zero values should contain a corresponding time unit (e.g. 1s, 2m, 3h). A value of zero means don't timeout requests. (default "0")
      --kube-server string                  The address and port of the Kubernetes API server
      --kube-tls-server-name string         If provided, this name will be used to validate server certificate. If this is not provided, hostname used to contact the server is used.
      --kube-token string                   Bearer token for authentication to the API server
      --kube-user string                    The name of the kubeconfig user to use
      --kube-username string                Username for basic authentication to the API server
      --version string                      Core instance version
      --wait                                Wait for the core instance to be ready before returning

Global Flags:
      --cloud-url string   Calyptia Cloud URL (default "https://cloud-api.calyptia.com")
      --token string       Calyptia Cloud Project token (default "xxx")
```

First set token which can be retrieved from the https://core.calyptia.com under "Settings"

```bash
calyptia config set_token xxx
```

Install operator into cluster.  Check [here](components.md) for details about core-operator.
This command will install core-operator into cluster's default namespace to use other namespace use `--kube-namespace` parmeter. 
```bash
calyptia install operator 
```

Install core instance. Check [here](components.md) for details about core instance
```bash
calyptia create core_instance operator --name <name-of-your-instance> --wait
```

The following output indicates that the core instance has been successfully installed:
```bash
Waiting for core operator to be ready...
Core operator is ready. Took 2.319019ms
Core instance created successfully
Resources created:
Deployment=dfasdfa-sync
Secret=calyptia-dfasdfa-default-secret
ClusterRole=calyptia-dfasdfa-default-cluster-role
ClusterRoleBinding=calyptia-dfasdfa-default-cluster-role-binding
ServiceAccount=calyptia-dfasdfa-default-service-account
```
Note: resource names will be different for every cluster.
For details about all kubernetes resources that will be created with installation process please check [here](manifest.md)
### Helm 

Not available yet

### Install using manifest

```bash 
kubectl apply -f https://github.com/calyptia/core-operator/releases/download/v1.0.0-RC1/manifest.yaml
```

## Creating first pipeline

After installing both the operator and the sync Pipelines can be created using the UI or Calyptia CLI.

## CLI

Creating pipelines

```bash
Create a new pipeline

Usage:
  calyptia create pipeline [flags]

Flags:
      --auto-create-ports         Automatically create pipeline ports from config (default true)
      --config-file string        Fluent Bit config file used by pipeline (default "fluent-bit.conf")
      --config-format string      Default configuration format to use (yaml, ini(deprecated))
      --core-instance string      Parent core-instance ID or name
      --encrypt-files             Encrypt file contents
      --environment string        Calyptia environment name
      --file stringArray          Optional file. You can reference this file contents from your config like so:
                                  {{ files.myfile }}
                                  Pass as many as you want; bear in mind the file name can only contain alphanumeric characters.
  -h, --help                      help for pipeline
      --image string              Fluent-bit docker image
      --metadata strings          Metadata to attach to the pipeline in the form of key:value. You could instead use a file with the --metadata-file option
      --metadata-file string      Metadata JSON file to attach to the pipeline intead of passing multiple --metadata flags
      --name string               Pipeline name; leave it empty to generate a random name
  -o, --output-format string      Output format. Allowed: table, json, yaml, go-template, go-template-file (default "table")
      --replicas uint             Pipeline replica size (default 1)
      --resource-profile string   Resource profile name (default "best-effort-low-resource")
      --secrets-file string       Optional file where secrets are defined. You can store key values and reference them inside your config like so:
                                  {{ secrets.foo }}
      --secrets-format string     Secrets file format. Allowed: auto, env, json, yaml. Auto tries to detect it from file extension (default "auto")
      --skip-config-validation    Opt-in to skip config validation (Use with caution as this option might be removed soon)
      --template string           Template string or path to use when -o=go-template, -o=go-template-file. The template format is golang templates
                                  [http://golang.org/pkg/text/template/#pkg-overview]

Global Flags:
      --cloud-url string   Calyptia Cloud URL (default "https://cloud-api.calyptia.com")
      --token string       Calyptia Cloud Project token (default "xxx")
```
First obtain desired core instance using CLI
```
calyptia get core_instances
```

```
NAME                    VERSION ENVIRONMENT PIPELINES TAGS STATUS  AGE
test-client             v1.1.6  default     3         test running 5 weeks
```

Create a configuration file:
```bash
cat >> cfg.yaml << 'END'
pipeline:
    inputs:
      - name: dummy
        tag: dummy.log
    outputs:
      - name: nrlogs
        match: '*'
        api_key: 1234
END
```


```bash
calyptia create pipeline --core-instance test-client --config-file cfg.yaml 
```
Result
```bash
ID                                   NAME                      AGE
2481080a-8a31-404d-96e3-8f9db9327fa0 magical-misty-knight-e8ce Just now
```

### Using kubectl

Create Pipeline using kubectl:
```yaml
cat <<EOF | kubectl apply -f -
apiVersion: core.calyptia.com/v1
kind: Pipeline
metadata:
  name: in-http
  namespace: default
spec:
  kind: deployment
  fluentbit:
    config: |-
      pipeline:
        inputs:
          - Name: dummy
            Tag: dummy.log
        outputs:
          - Name: http
            Match: '*'
            Host: out-http
            Port: 8888
            Format: json
EOF
```

```bash
pipeline.core.calyptia.com/in-http created
```

List pipelines in all namespaces
```bash
kubectl get pipeline -A
```
```bash
NAME                        STATUS
in-http                     STARTED
```

For detailes about Pipeline resource please visit [CRD Reference](crd-reference.md)