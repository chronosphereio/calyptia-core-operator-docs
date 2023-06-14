
- [Installing Calyptia Core](#installing-calyptia-core)
    - [Components](#components)
    - [Installation options](#installation-options)
        - [Calyptia CLI](#calyptia-cli)
        - [Helm](#helm)
        - [Manifest](#manifest)
- [CRD reference](./crd-reference.md)
- [Manifest](manifest.md)

# Installing Calyptia Core

1. Running Kubernetes cluster
2. Kubectl 

## Components

Calyptia Core consists from two components:

1. Core operator 

Core operator extends Kubernets cluster with Pipeline Custom Resource Definition and handles its lifecycle. Pipeline is an abstraction which contains all necessary information to run a Fluentbit instance 

2. Core instance

Core instance is a set of daemons running in your cluster which have several functions: 
* Registers itself with Cloud API 
* Fetches Pipelines created in the UI and syncs them with the cluster. Effectively creating or deleting Pipeline resources in the cluster
* Updates Pipelines status in the Cloud API 


## Installation options

### Calyptia CLI 

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

Recommended installation:
```bash
$ calyptia config set_token xxx
$ calyptia install operator 
```
Alternatively operator can be installed:
```bash
$ export TOKEN=xxx
$ calyptia install operator
```
or 
```bash
$ calyptia install core --token xxx
```

### Helm 
TBA - currently not available

### Manifest

``` 
$ kubectl apply -f https://github.com/calyptia/core-operator/releases/download/v1.0.0-alpha4/manifest.yaml
```


