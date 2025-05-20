# linode-cli-stuff

Prereqs: jq

Useful links: https://github.com/linode/linode-cli/wiki/Usage


## install/upgrade with pip
```
pip3 install --upgrade linode-cli
```



## cli configuration
```
 ~ % linode-cli configure
Welcome to the Linode CLI.  This will walk you through some initial setup.
The CLI will use its web-based authentication to log you in.
If you prefer to supply a Personal Access Token,use `linode-cli configure --token`.
Press enter to continue. This will open a browser and proceed with authentication.
```



## LKE

### Create LKE Cluster

```
# Region/Cluster_Name/k8s version
export LINODE_REGION="fr-par"
export LKE_CLUSTER_NAME=sbenfred-lke-001
export K8S_VER="1.31"
 
# [optional]
# If a token is being used versus authentication
# export LINODE_TOKEN=xxx
 
# Create cluster with 3 nodes/no HA/shared CPU 16G (g6-standard-6)
linode-cli lke cluster-create --label $LKE_CLUSTER_NAME --tags TEST --region $LINODE_REGION --k8s_version $K8S_VER --node_pools.type g6-standard-6 --control_plane.high_availability false --control_plane.acl.enabled true --control_plane.acl.addresses.ipv4 "<myIP>/32" --node_pools.count 3
```



### Retrieve ClusterID
```
CLUSTER_ID=$(linode lke clusters-list --json | \
    jq -r --arg lke_cluster_name "$LKE_CLUSTER_NAME" \
      '.[] | select(.label == $lke_cluster_name) | .id')
```



### Retrieve kubeconfig for a given ClusterID
```
linode lke kubeconfig-view --json "$CLUSTER_ID" | \
    jq -r '.[0].kubeconfig' | \
    base64 --decode > ~/myrepo/lke-kubeconfig.yaml
```



## Volumes

```
linode-cli volumes list

linode-cli volumes delete 7831513
```



## linode commands
```
linode-cli commands


CLI user management commands:
┌─────────────┬──────────┬────────────┐
│ configure   │ set-user │ show-users │
│ remove-user │          │            │
└─────────────┴──────────┴────────────┘

CLI Plugin management commands:
┌─────────────────┬───────────────┐
│ register-plugin │ remove-plugin │
└─────────────────┴───────────────┘

Other CLI commands:
┌────────────┐
│ completion │
└────────────┘

Available commands:
┌───────────────┬────────────────────┬───────────────────┐
│ account       │ betas              │ child-account     │
│ databases     │ domains            │ events            │
│ firewalls     │ images             │ kernels           │
│ linodes       │ lke                │ longview          │
│ managed       │ network-transfer   │ networking        │
│ nodebalancers │ object-storage     │ payment-methods   │
│ phone         │ placement          │ profile           │
│ regions       │ security-questions │ service-transfers │
│ sshkeys       │ stackscripts       │ tags              │
│ tickets       │ users              │ vlans             │
│ volumes       │ vpcs               │                   │
└───────────────┴────────────────────┴───────────────────┘
```


