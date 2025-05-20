# linode-cli-stuff


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
 
# Create cluster with 3 nodes/no HA/shared CPU 16G (g6-standard-6) - currently max flavor allowed for SE
linode-cli lke cluster-create --label $LKE_CLUSTER_NAME --tags TEST_HELM_ESO --region $LINODE_REGION --k8s_version $K8S_VER --node_pools.type g6-standard-6 --control_plane.high_availability false --control_plane.acl.enabled true --control_plane.acl.addresses.ipv4 "<myIP>/32" --node_pools.count 3
```