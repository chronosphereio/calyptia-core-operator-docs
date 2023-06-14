# API Reference

Packages:

- [core.calyptia.com/v1](#corecalyptiacomv1)

# core.calyptia.com/v1

Resource Types:

- [Pipeline](#pipeline)




## Pipeline
<sup><sup>[↩ Parent](#corecalyptiacomv1 )</sup></sup>






Pipeline is the Schema for the pipelines API

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
      <td><b>apiVersion</b></td>
      <td>string</td>
      <td>core.calyptia.com/v1</td>
      <td>true</td>
      </tr>
      <tr>
      <td><b>kind</b></td>
      <td>string</td>
      <td>Pipeline</td>
      <td>true</td>
      </tr>
      <tr>
      <td><b><a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.20/#objectmeta-v1-meta">metadata</a></b></td>
      <td>object</td>
      <td>Refer to the Kubernetes API documentation for the fields of the `metadata` field.</td>
      <td>true</td>
      </tr><tr>
        <td><b><a href="#pipelinespec">spec</a></b></td>
        <td>object</td>
        <td>
          PipelineSpec defines the desired state of Pipeline<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#pipelinestatus">status</a></b></td>
        <td>object</td>
        <td>
          PipelineStatus defines the observed state of Pipeline<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.spec
<sup><sup>[↩ Parent](#pipeline)</sup></sup>



PipelineSpec defines the desired state of Pipeline

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>kind</b></td>
        <td>string</td>
        <td>
          Enums<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>cloudResourceID</b></td>
        <td>string</td>
        <td>
          CloudResourceID represents the ID under which pipeline exists in Cloud API<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#pipelinespecfluentbit">fluentbit</a></b></td>
        <td>object</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>image</b></td>
        <td>string</td>
        <td>
          <br/>
          <br/>
            <i>Default</i>: ghcr.io/calyptia/core/calyptia-fluent-bit:23.2.3<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#pipelinespecportsindex">ports</a></b></td>
        <td>[]object</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>replicasCount</b></td>
        <td>integer</td>
        <td>
          <br/>
          <br/>
            <i>Format</i>: int32<br/>
            <i>Default</i>: 1<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#pipelinespecresources">resources</a></b></td>
        <td>object</td>
        <td>
          ResourceProfile model.<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.spec.fluentbit
<sup><sup>[↩ Parent](#pipelinespec)</sup></sup>





<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>config</b></td>
        <td>string</td>
        <td>
          Fluentbit config in YAML format
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#pipelinespecfluentbitfilesindex">files</a></b></td>
        <td>[]object</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>parsers</b></td>
        <td>string</td>
        <td>
          Fluentbit parsers in YAML format
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#pipelinespecfluentbitsecretsindex">secrets</a></b></td>
        <td>[]object</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.spec.fluentbit.files[index]
<sup><sup>[↩ Parent](#pipelinespecfluentbit)</sup></sup>





<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>content</b></td>
        <td>string</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>name</b></td>
        <td>string</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.spec.fluentbit.secrets[index]
<sup><sup>[↩ Parent](#pipelinespecfluentbit)</sup></sup>





<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>key</b></td>
        <td>string</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>name</b></td>
        <td>string</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.spec.ports[index]
<sup><sup>[↩ Parent](#pipelinespec)</sup></sup>





<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>backendPort</b></td>
        <td>integer</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>cloudResourceID</b></td>
        <td>string</td>
        <td>
          CloudResourceID represents the ID under which pipeline port exists in Cloud API<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>frontendPort</b></td>
        <td>integer</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>port</b></td>
        <td>integer</td>
        <td>
          <br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>protocol</b></td>
        <td>string</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.spec.resources
<sup><sup>[↩ Parent](#pipelinespec)</sup></sup>



ResourceProfile model.

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b><a href="#pipelinespecresourceslimits">limits</a></b></td>
        <td>object</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#pipelinespecresourcesrequests">requests</a></b></td>
        <td>object</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.spec.resources.limits
<sup><sup>[↩ Parent](#pipelinespecresources)</sup></sup>





<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>cpu</b></td>
        <td>string</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>memory</b></td>
        <td>string</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.spec.resources.requests
<sup><sup>[↩ Parent](#pipelinespecresources)</sup></sup>





<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>cpu</b></td>
        <td>string</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>memory</b></td>
        <td>string</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#pipelinespecresourcesrequestsstorage">storage</a></b></td>
        <td>object</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.spec.resources.requests.storage
<sup><sup>[↩ Parent](#pipelinespecresourcesrequests)</sup></sup>





<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>backlog-mem-limit</b></td>
        <td>string</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>max-chunks-pause</b></td>
        <td>boolean</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>max-chunks-up</b></td>
        <td>integer</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>sync-full</b></td>
        <td>boolean</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>volume-size</b></td>
        <td>string</td>
        <td>
          <br/>
          <br/>
            <i>Default</i>: 256Mi<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.status
<sup><sup>[↩ Parent](#pipeline)</sup></sup>



PipelineStatus defines the observed state of Pipeline

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>status</b></td>
        <td>string</td>
        <td>
          PipelineStatusKind enum.<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b><a href="#pipelinestatusdaemonsetstatus">daemonsetstatus</a></b></td>
        <td>object</td>
        <td>
          DaemonSetStatus represents the current status of a daemon set.<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#pipelinestatusdeploymentstatus">deploymentstatus</a></b></td>
        <td>object</td>
        <td>
          DeploymentStatus is the most recently observed status of the Deployment.<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#pipelinestatusservicestatuseskey">servicestatuses</a></b></td>
        <td>map[string]object</td>
        <td>
          <br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.status.daemonsetstatus
<sup><sup>[↩ Parent](#pipelinestatus)</sup></sup>



DaemonSetStatus represents the current status of a daemon set.

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>currentNumberScheduled</b></td>
        <td>integer</td>
        <td>
          The number of nodes that are running at least 1 daemon pod and are supposed to run the daemon pod. More info: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>desiredNumberScheduled</b></td>
        <td>integer</td>
        <td>
          The total number of nodes that should be running the daemon pod (including nodes correctly running the daemon pod). More info: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>numberMisscheduled</b></td>
        <td>integer</td>
        <td>
          The number of nodes that are running the daemon pod, but are not supposed to run the daemon pod. More info: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>numberReady</b></td>
        <td>integer</td>
        <td>
          numberReady is the number of nodes that should be running the daemon pod and have one or more of the daemon pod running with a Ready Condition.<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>collisionCount</b></td>
        <td>integer</td>
        <td>
          Count of hash collisions for the DaemonSet. The DaemonSet controller uses this field as a collision avoidance mechanism when it needs to create the name for the newest ControllerRevision.<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#pipelinestatusdaemonsetstatusconditionsindex">conditions</a></b></td>
        <td>[]object</td>
        <td>
          Represents the latest available observations of a DaemonSet's current state.<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>numberAvailable</b></td>
        <td>integer</td>
        <td>
          The number of nodes that should be running the daemon pod and have one or more of the daemon pod running and available (ready for at least spec.minReadySeconds)<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>numberUnavailable</b></td>
        <td>integer</td>
        <td>
          The number of nodes that should be running the daemon pod and have none of the daemon pod running and available (ready for at least spec.minReadySeconds)<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>observedGeneration</b></td>
        <td>integer</td>
        <td>
          The most recent generation observed by the daemon set controller.<br/>
          <br/>
            <i>Format</i>: int64<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>updatedNumberScheduled</b></td>
        <td>integer</td>
        <td>
          The total number of nodes that are running updated daemon pod<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.status.daemonsetstatus.conditions[index]
<sup><sup>[↩ Parent](#pipelinestatusdaemonsetstatus)</sup></sup>



DaemonSetCondition describes the state of a DaemonSet at a certain point.

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>status</b></td>
        <td>string</td>
        <td>
          Status of the condition, one of True, False, Unknown.<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>type</b></td>
        <td>string</td>
        <td>
          Type of DaemonSet condition.<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>lastTransitionTime</b></td>
        <td>string</td>
        <td>
          Last time the condition transitioned from one status to another.<br/>
          <br/>
            <i>Format</i>: date-time<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>message</b></td>
        <td>string</td>
        <td>
          A human readable message indicating details about the transition.<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>reason</b></td>
        <td>string</td>
        <td>
          The reason for the condition's last transition.<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.status.deploymentstatus
<sup><sup>[↩ Parent](#pipelinestatus)</sup></sup>



DeploymentStatus is the most recently observed status of the Deployment.

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>availableReplicas</b></td>
        <td>integer</td>
        <td>
          Total number of available pods (ready for at least minReadySeconds) targeted by this deployment.<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>collisionCount</b></td>
        <td>integer</td>
        <td>
          Count of hash collisions for the Deployment. The Deployment controller uses this field as a collision avoidance mechanism when it needs to create the name for the newest ReplicaSet.<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#pipelinestatusdeploymentstatusconditionsindex">conditions</a></b></td>
        <td>[]object</td>
        <td>
          Represents the latest available observations of a deployment's current state.<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>observedGeneration</b></td>
        <td>integer</td>
        <td>
          The generation observed by the deployment controller.<br/>
          <br/>
            <i>Format</i>: int64<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>readyReplicas</b></td>
        <td>integer</td>
        <td>
          readyReplicas is the number of pods targeted by this Deployment with a Ready Condition.<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>replicas</b></td>
        <td>integer</td>
        <td>
          Total number of non-terminated pods targeted by this deployment (their labels match the selector).<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>unavailableReplicas</b></td>
        <td>integer</td>
        <td>
          Total number of unavailable pods targeted by this deployment. This is the total number of pods that are still required for the deployment to have 100% available capacity. They may either be pods that are running but not yet available or pods that still have not been created.<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>updatedReplicas</b></td>
        <td>integer</td>
        <td>
          Total number of non-terminated pods targeted by this deployment that have the desired template spec.<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.status.deploymentstatus.conditions[index]
<sup><sup>[↩ Parent](#pipelinestatusdeploymentstatus)</sup></sup>



DeploymentCondition describes the state of a deployment at a certain point.

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>status</b></td>
        <td>string</td>
        <td>
          Status of the condition, one of True, False, Unknown.<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>type</b></td>
        <td>string</td>
        <td>
          Type of deployment condition.<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>lastTransitionTime</b></td>
        <td>string</td>
        <td>
          Last time the condition transitioned from one status to another.<br/>
          <br/>
            <i>Format</i>: date-time<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>lastUpdateTime</b></td>
        <td>string</td>
        <td>
          The last time this condition was updated.<br/>
          <br/>
            <i>Format</i>: date-time<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>message</b></td>
        <td>string</td>
        <td>
          A human readable message indicating details about the transition.<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>reason</b></td>
        <td>string</td>
        <td>
          The reason for the condition's last transition.<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.status.servicestatuses[key]
<sup><sup>[↩ Parent](#pipelinestatus)</sup></sup>



ServiceStatus represents the current status of a service.

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b><a href="#pipelinestatusservicestatuseskeyconditionsindex">conditions</a></b></td>
        <td>[]object</td>
        <td>
          Current service state<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#pipelinestatusservicestatuseskeyloadbalancer">loadBalancer</a></b></td>
        <td>object</td>
        <td>
          LoadBalancer contains the current status of the load-balancer, if one is present.<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.status.servicestatuses[key].conditions[index]
<sup><sup>[↩ Parent](#pipelinestatusservicestatuseskey)</sup></sup>



Condition contains details for one aspect of the current state of this API Resource. --- This struct is intended for direct use as an array at the field path .status.conditions.  For example, 
 type FooStatus struct{ // Represents the observations of a foo's current state. // Known .status.conditions.type are: "Available", "Progressing", and "Degraded" // +patchMergeKey=type // +patchStrategy=merge // +listType=map // +listMapKey=type Conditions []metav1.Condition `json:"conditions,omitempty" patchStrategy:"merge" patchMergeKey:"type" protobuf:"bytes,1,rep,name=conditions"` 
 // other fields }

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>lastTransitionTime</b></td>
        <td>string</td>
        <td>
          lastTransitionTime is the last time the condition transitioned from one status to another. This should be when the underlying condition changed.  If that is not known, then using the time when the API field changed is acceptable.<br/>
          <br/>
            <i>Format</i>: date-time<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>message</b></td>
        <td>string</td>
        <td>
          message is a human readable message indicating details about the transition. This may be an empty string.<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>reason</b></td>
        <td>string</td>
        <td>
          reason contains a programmatic identifier indicating the reason for the condition's last transition. Producers of specific condition types may define expected values and meanings for this field, and whether the values are considered a guaranteed API. The value should be a CamelCase string. This field may not be empty.<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>status</b></td>
        <td>enum</td>
        <td>
          status of the condition, one of True, False, Unknown.<br/>
          <br/>
            <i>Enum</i>: True, False, Unknown<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>type</b></td>
        <td>string</td>
        <td>
          type of condition in CamelCase or in foo.example.com/CamelCase. --- Many .condition.type values are consistent across resources like Available, but because arbitrary conditions can be useful (see .node.status.conditions), the ability to deconflict is important. The regex it matches is (dns1123SubdomainFmt/)?(qualifiedNameFmt)<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>observedGeneration</b></td>
        <td>integer</td>
        <td>
          observedGeneration represents the .metadata.generation that the condition was set based upon. For instance, if .metadata.generation is currently 12, but the .status.conditions[x].observedGeneration is 9, the condition is out of date with respect to the current state of the instance.<br/>
          <br/>
            <i>Format</i>: int64<br/>
            <i>Minimum</i>: 0<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.status.servicestatuses[key].loadBalancer
<sup><sup>[↩ Parent](#pipelinestatusservicestatuseskey)</sup></sup>



LoadBalancer contains the current status of the load-balancer, if one is present.

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b><a href="#pipelinestatusservicestatuseskeyloadbalanceringressindex">ingress</a></b></td>
        <td>[]object</td>
        <td>
          Ingress is a list containing ingress points for the load-balancer. Traffic intended for the service should be sent to these ingress points.<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.status.servicestatuses[key].loadBalancer.ingress[index]
<sup><sup>[↩ Parent](#pipelinestatusservicestatuseskeyloadbalancer)</sup></sup>



LoadBalancerIngress represents the status of a load-balancer ingress point: traffic intended for the service should be sent to an ingress point.

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>hostname</b></td>
        <td>string</td>
        <td>
          Hostname is set for load-balancer ingress points that are DNS based (typically AWS load-balancers)<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b>ip</b></td>
        <td>string</td>
        <td>
          IP is set for load-balancer ingress points that are IP based (typically GCE or OpenStack load-balancers)<br/>
        </td>
        <td>false</td>
      </tr><tr>
        <td><b><a href="#pipelinestatusservicestatuseskeyloadbalanceringressindexportsindex">ports</a></b></td>
        <td>[]object</td>
        <td>
          Ports is a list of records of service ports If used, every port defined in the service should have an entry in it<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>


### Pipeline.status.servicestatuses[key].loadBalancer.ingress[index].ports[index]
<sup><sup>[↩ Parent](#pipelinestatusservicestatuseskeyloadbalanceringressindex)</sup></sup>





<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Type</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
    </thead>
    <tbody><tr>
        <td><b>port</b></td>
        <td>integer</td>
        <td>
          Port is the port number of the service port of which status is recorded here<br/>
          <br/>
            <i>Format</i>: int32<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>protocol</b></td>
        <td>string</td>
        <td>
          Protocol is the protocol of the service port of which status is recorded here The supported values are: "TCP", "UDP", "SCTP"<br/>
          <br/>
            <i>Default</i>: TCP<br/>
        </td>
        <td>true</td>
      </tr><tr>
        <td><b>error</b></td>
        <td>string</td>
        <td>
          Error is to record the problem with the service port The format of the error shall comply with the following rules: - built-in error values shall be specified in this file and those shall use CamelCase names - cloud provider specific error values must have names that comply with the format foo.example.com/CamelCase. --- The regex it matches is (dns1123SubdomainFmt/)?(qualifiedNameFmt)<br/>
        </td>
        <td>false</td>
      </tr></tbody>
</table>