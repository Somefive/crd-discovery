# CRD Discovery

CRD Discovery is a discovery tool for Kubernetes CRDs between multiple clusters.

## Architecture

CRD Discovery have two components, one is Web Server deployed in master K8s, another is a kubectl plugin. So if one use kubectl to apply config with some CRD that the destination cluster don't have, then the user can use kubectl plugin to discovry this CRD from web server.

### Web Server

This should be a deployment running on master K8s cluster. This web server has two main function.

1. Serve kubectl requests who may need to create CRD to master K8s cluster.
2. Serve kubectl requests who may want to sync CRD from master K8s cluster.

Once the web server received the CRD create request. It can apply the CRD to the master K8s cluster. On the other hand, CRD sync request can also load CRD from the master K8s cluster. So the web server don't need to store anything.


### Kubectl plugin

Kubectl plugin is the [extensions mechanism for kubectl](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/).

After writing this plugin, we may use kubectl like below:

1. list crd from master K8s

    ```
    kubectl syncrd list
    ```

2. get crd describe from master K8s

    ```
    kubectl syncrd get crd-example1
    ```

2. sync all crd from master K8s to the destination K8s cluster. The destination K8s cluster was defined in kube config.

    ```
    kubectl syncrd sync
    ```

3. sync one specify crd from master K8s to the destination K8s cluster.

    ```
    kubectl syncrd sync crd-example
    ```

4. create CRD from local to master

    ```
    kubectl syncrd create -f crd.yaml
    ```

