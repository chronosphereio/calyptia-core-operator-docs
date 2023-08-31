# Updating

**NOTE:*** avaialble in versions v1.7.0+

## CLI Updating Operator image

How to update operator image using calyptia CLI:

```shel
# calyptia update operator --version 1.0.15
Waiting for core-operator to update...
Core operator manager successfully updated to version v1.0.15
```

* `version` - parameter expects semver version of string like 1.0.15, v1.0.15 and other inputs will be rejected

Incorrect version format:

```shell
# calyptia update operator --version 11.fw.2
Error: Malformed version: v11.fw.2
```

In the case of non-existent image tag the CLI will return:

```shell
# calyptia update operator --version v1.0.155
Waiting for core-operator to update...
Error: could not update core-operator to version v1.0.155 for extra details use --verbose flag
```

Verbose output:

```shell
# calyptia update operator --version v1.0.155 --verbose
Waiting for core-operator to update...
Error: could not update core-operator to version v1.0.155, 
failed while waiting for deployment to start:
* pod calyptia-core-controller-manager-6fd8867b6f-zgzgc, Message: rpc error: code = NotFound desc = failed to pull and unpack image "ghcr.io/calyptia/core-operator:v1.0.155": failed to resolve reference "ghcr.io/calyptia/core-operator:v1.0.155": ghcr.io/calyptia/core-operator:v1.0.155: not found'
```

## CLI Updating core-instance image

How to update core-instance image using calyptia CLI:

```shell
# calyptia update core_instance operator refined-siren-d0db --version 1.0.15
Waiting for core-instance to update...
calyptia-core instance version updated to version v1.0.15
```

* `version` - parameter expects semver version of string like 1.0.15, v1.0.15 and other inputs will be rejected

Incorrect version format:

```shell
# calyptia update operator --version 11.fw.2
Error: Malformed version: v11.fw.2
```

In the case of non-existent image tag the CLI will return:

```shell
# calyptia update core_instance operator refined-siren-d0db --version 1.0.155
Waiting for core-instance to update...
Error: could not update core-instance to version v1.0.155 for extra details use --verbose flag
```

Verbose output:

```shell
# calyptia update core_instance operator refined-siren-d0db --version 1.0.155 --verbose
Waiting for core-instance to update...
Error: could not update core-instance: to version v1.0.155 failed while waiting for deployment to start:
* pod calyptia-refined-siren-d0db-default-sync-56798647fb-q6wls, Message: rpc error: code = NotFound desc = failed to pull and unpack image "ghcr.io/calyptia/core-operator/sync-from-cloud:v1.0.155": failed to resolve reference "ghcr.io/calyptia/core-operator/sync-from-cloud:v1.0.155": ghcr.io/calyptia/core-operator/sync-from-cloud:v1.0.155: not found
rpc error: code = NotFound desc = failed to pull and unpack image "ghcr.io/calyptia/core-operator/sync-to-cloud:v1.0.155": failed to resolve reference "ghcr.io/calyptia/core-operator/sync-to-cloud:v1.0.155": ghcr.io/calyptia/core-operator/sync-to-cloud:v1.0.155: not found'
```

## Helm - update core-operator image

```shell
helm upgrade core-operator calyptia/core-operator --set operatorImage=ghcr.io/calyptia/core-operator:v1.0.15
```

## Helm - update core-instance image

```shell
helm upgrade core-operator calyptia/core-instance --set fromcloudImage=ghcr.io/calyptia/core-operator/sync-from-cloud:v1.0.15 --set tocloudImage=ghcr.io/calyptia/core-operator/sync-to-cloud:v1.0.15
```

List of available versions can be obtained [here](https://github.com/calyptia/core-operator/tags) (the link isn't publicly available)
