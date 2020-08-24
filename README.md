# Deploying the Capsule Operator
Use the Capsule Operator for easily implementing, managing, and maintaining mutitenancy and access control in Kubernetes.

## Requirements
* [Helm 3](https://github.com/helm/helm/releases) is required when installing the Capsule Operator chart. Follow Helm’s official [steps](https://helm.sh/docs/intro/install/) for installing helm on your particular operating system.

* A Kubernetes cluster 1.16+ with following [Admission Controllers](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/) enabled:

    * PodNodeSelector
    * LimitRanger
    * ResourceQuota
    * MutatingAdmissionWebhook
    * ValidatingAdmissionWebhook

* A [`kubeconfig`](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) file accessing the Kubernetes cluster with cluster admin permissions.

## Quick Start
The Capsule Operator Chart can be used to instantly deploy the Capsule Operator on your Kubernetes cluster.

1. Clone this repo:

        $ git clone https://github.com/clastix/capsule-helm-chart

2. Install the Chart:

        $ helm install capsule capsule-helm-chart -n capsule-system

3. Show the status:

        $ helm status capsule -n capsule-system

4. Upgrade the Chart

        $ helm upgrade capsule -n capsule-system

5. Uninstall the Chart
   
        $ helm uninstall capsule -n capsule-system

## Customize the installation
There are two methods for specifying overrides of values during chart installation: `--values` and `--set`.

The `--values` option is the preferred method because it allows you to keep your overrides in a YAML file, rather than specifying them all on the command line. Create a copy of the YAML file `values.yaml` and add your overrides to it.

Specify your overrides file when you install the chart:

        $ helm install capsule capsule-helm-chart --values myvalues.yaml -n capsule-system   

The values in your overrides file `myvalues.yaml` will override their counterparts in the chart’s values.yaml file. Any values in `values.yaml` that weren’t overridden will keep their defaults.

If you only need to make minor customizations, you can specify them on the command line by using the `--set` option. For example:

        $ helm install capsule capsule-helm-chart --set force_tenant_prefix=false -n capsule-system

Here the values you can override:

Parameter | Description | Default
--- | --- | ---
`namespace.create` | Specifies whether a custom namespace should be created. If false default to `capsule-system`. | `false`
`namespace.name` | Currently is not yet supported. | `null`
`force_tenant_prefix` | When set, all namespaces created in the tenant must follow the following naming convention: *tanant-namespace* otherwise the namespace creation is denied by the controller. This is a global setting valid for all tenants.| `false`
`log_level` | Set the log verbosity of the controller with a value from 1 to 10.| `4`
`manager.image.repository` | Set the image repository of the controller. | `quay.io/clastix/capsule`
`manager.image.tag` | Overrides the image tag whose default is the chart. `appVersion` | `null`
`manager.image.pullPolicy` | Set the image pull policy. | `IfNotPresent`
`manager.resources.requests/cpu` | Set the CPU requests assigned to the controller. | `200m`
`manager.resources.requests/memory` | Set the memory requests assigned to the controller. | `128Mi`
`manager.resources.limits/cpu` | Set the CPU limits assigned to the controller. | `200m`
`manager.resources.limits/cpu` | Set the memory limits assigned to the controller. | `128Mi`
`proxy.image.repository` | Set the image repository of the rbac proxy. | `gcr.io/kubebuilder/kube-rbac-proxy`
`proxy.image.tag` | Set the image tag of the rbac proxy. | `v0.5.0`
`proxy.image.pullPolicy` | Set the image pull policy. | `IfNotPresent`
`proxy.resources.requests/cpu` | Set the CPU requests assigned to the rbac proxy. | `10m`
`proxy.resources.requests/memory` | Set the memory requests assigned to the rbac proxy. | `64Mi`
`proxy.resources.limits/cpu` | Set the CPU limits assigned to the rbac proxy. | `100m`
`proxy.resources.limits/cpu` | Set the memory limits assigned to the rbac proxy. | `128Mi`
`imagePullSecrets` | Configuration for `imagePullSecrets` so that you can use a private images registry. | `[]`
`serviceAccount.create` | Specifies whether a service account should be created. | `true`
`serviceAccount.annotations` | Annotations to add to the service account. | `{}`
`serviceAccount.name` | The name of the service account to use. If not set and `serviceAccount.create=true`, a name is generated using the fullname template | `capsule`
`podAnnotations` | Annotations to add to the Capsule pod. | `{}`
`priorityClassName` | Set the priority class name of the Capsule pod. | `null`
`nodeSelector` | Set the node selector for the Capsule pod. | `{}`
`tolerations` | Set list of tolerations for the Capsule pod. | `[]`
`replicaCount` | Set the replica count for Capsule pod. | `1`
`affinity` | Set affinity rules for the Capsule pod. | `{}`
`podSecurityPolicy.enabled` | Specify if a Pod Security Policy must be created. | `false`


## More
See Capsule [use cases](https://github.com/clastix/capsule/blob/master/use_cases.md) for more information about how to use Capsule.

