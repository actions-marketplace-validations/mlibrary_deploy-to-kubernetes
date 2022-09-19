# deploy-to-kubernetes
Action for deploying a ghcr image to a kuberenetes cluster

Uses [kubectl set image](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-image-em-) command.

## Example
To use this action to deploy a deployment image to the `web` deployment and `web` container:
```
   - name: Deploy to NAMESPACE
      uses: mlibrary/deploy-to-kubernetes@v3
      with:
        registry_token: ${{ secrets.GITHUB_TOKEN }}
        image: ghcr.io/myorganization/my_app:latest
        cluster_ca: ${{ secrets.KUBERNETES_CA }}
        cluster_server: my-kubernetes-server
        namespace_token: ${{ secrets.NAMESPACE_CA }}
        namespace: my-app-namespace
       
```

To use this action to deploy a cronjob image
```
   - name: Deploy to NAMESPACE
      uses: mlibrary/deploy-to-kubernetes@v3
      with:
        registry_token: ${{ secrets.GITHUB_TOKEN }}
        image: ghcr.io/myorganization/my_app:latest
        cluster_ca: ${{ secrets.KUBERNETES_CA }}
        cluster_server: my-kubernetes-server
        namespace_token: ${{ secrets.NAMESPACE_CA }}
        namespace: my-app-namespace
        resource_type: cronjob
        resource_name: my-cronjob-name
        container: my-cronjob-name
       
```

## Inputs

### Required

| Name | Description | 
|----------|-------------|
| `registry_token` | The token to use to log in to the registry. For GHCR this shold be `${{ secrets.GITHUB_TOKEN }}`.|
| `image`          | Image to deploy, e.g. `registry/organization/image_name:tag` |
|`cluster_server`  | The Kubernetes server |
| `cluster_ca`     | The Kubernetes cluster CA certificate, base64-encoded, e.g. from: <br/> `cat ~/.kube/certs/my-cluster/k8s-ca.crt \| base64`  |
| `namespace_token`| A base64-encoded Kubernetes service account token that can be used to set the image for the given deployment in the given namespace, e.g. from: <br> `kubectl -n my-namespace get -o json secret my-service-token \| jq -r .data.token` |
|`namespace`       | The Kubernetes namespace to deploy to.  |

### Optional 

| Name | Description | Value |
|------|-------------|-------|
|`registry`| Registry to log in to | `ghcr.io` |
|`registry_username`|  Username to log into the registry with | `${{ github.actor }}` |
|`resource_type`| Any resource type, comma-separated set of resource types, or 'all'| `deployment` |
|`resource_name`| Name of the resource whose image to set, a label selector like `--selector='app=someapp'`, or `--all` | `web` |
|`container`| The container in the resource whose image to set, or "*" for all containers | `web` | 
