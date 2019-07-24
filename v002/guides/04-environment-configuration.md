# Environment Configuration
Podder.ai enables you to build a high scalable and stable system with few commands. All you need to do is just writing down envionment configuration.

## Initialize Configuration
You can initialize configuration by the following command.
```bash
$ podder cluster init
```

You will be asked your environment setting configuration, such as provider and cluster settings.
```bash
$ podder cluster init
Cluster name [podder]:
Cluster Provider [eks]:
Docker registry type [docker_hub]:
Terraform backend type (local, s3) [local]: s3
Setting up provider
AWS access key id:
AWS secret access key:
AWS Region [ap-southeast-1]:
Setting up cluster
Auto scaling worker group number [1]: 2
Please configure auto scaling worker group [1]
[1] Name [worker-group-1]:
[1] Min node number [1]:
[1] Max node number [1]: 3
[1] Instance type [a1.2xlarge]: t3.large
[1] Storage size (GB) [50]:
Please configure auto scaling worker group [2]
[2] Name [worker-group-2]:
[2] Min node number [1]:
[2] Max node number [1]: 3
[2] Instance type [a1.2xlarge]: t3.large
[2] Storage size (GB) [50]:
Please provide your Docker Hub information
Docker Hub username [username]:
Docker Hub email [email@sample.com]:
Docker Hub password [********]:
Docker Hub image namespace [username]:
Please provide your S3 backend for Terraform
S3 bucket name [podder-terraform-state]:
S3 path for state file [terraform/terraform.tfstate]:
S3 region [ap-southeast-1]:
```
After you input the all information `cluster.yml` is created under .podder directory.

## Terraform backend configuration

By default, Podder Cli Terraform will create files locally but also a remote storage may be used.
Most used storage is an AWS S3 bucket as a Terraform backend.
We provide 2 type of Terraform backend in this moment.
- local: store Terraform state to local files
- s3: use a S3 bucket storage of Terraform state

Please prepare a S3 buctket for below settings
You will be asked your environment setting configuration as below
```
Terraform backend type (local, s3) [local]: s3

Please provide your S3 backend for Terraform
S3 bucket name [podder-terraform-state]:
S3 path for state file [terraform/terraform.tfstate]:
S3 region [ap-southeast-1]:
```

After `podder cluster init`, you can edit directly `cluster.yml` is created under .podder directory.
```yaml
 terraform_settings:
  backend:
    type: s3
    bucket: podder-terraform-state
    key: terraform/terraform.tfstate
    region: ap-southeast-1
 ```

## Kubernetes Cluster autoscaler
### About Cluster autoscaler
Podder Cluster uses [Cluster Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler) for automatic adjustment of size of the kubernetes cluster, when one of the following conditions is true:
- There are pods that failed to run in the cluster, due to insufficient resources.
- There are nodes in the cluster that have been underutilized for an extended period of time and their pods can be placed on other existing nodes.

### How to configure Cluster AutoScaler on your own Cluster
Podder CLI uses `.podder/cluster.yml` for configuring Cluster AutoScaler. So please change your settings `.podder/cluster.yml` before running `podder cluster apply`.

If you have already run `podder cluster apply`, please remove line `cluster_autoscaler: True` from your `.podder/cluster.yml` and run `podder cluster apply` again.

### About Persistent Volumes provisioner

For EKS cluster, we will use Elastic File System (EFS) for ReadWriteMany persistent volumes. 

You need to add following permission for your Terraform IAM user before you initialize cluster configuration.
From AWS console Please add `AmazonElasticFileSystemFullAccess` to IAM user using for Terraform
Or from json editor, please add bellow content
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ec2:CreateNetworkInterface",
                "ec2:DeleteNetworkInterface",
                "ec2:DescribeAvailabilityZones",
                "ec2:DescribeNetworkInterfaceAttribute",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeSubnets",
                "ec2:DescribeVpcAttribute",
                "ec2:DescribeVpcs",
                "ec2:ModifyNetworkInterfaceAttribute",
                "elasticfilesystem:*",
                "kms:DescribeKey",
                "kms:ListAliases"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

We are working on Persistent Volumes provisioner for another provider, please setup your provisioner before deploment.

#### Sample of cluster.yml
```yaml
# Cluster autoscaler parameters
cluster_autoscaler:
 v: 4
 stderrthreshold: info
 logtostderr: true
 # write-status-configmap: true
 # leader-elect: true
 # expander: random
 balance-similar-node-groups: true
 scan-interval: 10s
 scale-down-enabled: true
 scale-down-delay-after-add: 10m
 scale-down-delay-after-delete: 10m
 scale-down-delay-after-failure: 3m
 scale-down-unneeded-time: 10m
 scale-down-utilization-threshold: 0.5
 scale-down-non-empty-candidates-count: 5
 # min-replica-count: 2
 # max-node-provision-time: 15m0s
 # skip-nodes-with-local-storage: false
 # skip-nodes-with-system-pods: true
 # skip-nodes-with-local-storage: false
 ```

#### Cluster AutoScaler parameters
The following startup parameters are supported for cluster autoscaler.

| Parameter | Description | Default |
| --- | --- | --- |
| `scale-down-enabled` | Should CA scale down the cluster. | true |
| `scale-down-delay-after-add` | How long after scale up, that scale down evaluation resumes. | 10 minutes |
| `scale-down-delay-after-delete` | How long after node deletion that scale down evaluation resumes, defaults to scan-interval. | scan-interval |
| `scale-down-delay-after-failure` | How long after scale down failure that scale down evaluation resumes. | 3 minutes |
| `scale-down-unneeded-time` | How long a node should be unneeded before. | 10 minutes |
| `scale-down-unready-time` | How long an unready node should unneeded before it is eligible for scale down. | 20 minutes |
| `scale-down-utilization-threshold` | Node utilization level, defined as sum of requested resources divided by capacity, below which a node can be considered for scale down. | 0.5 |
| `scale-down-non-empty-candidates-count` | Maximum number of non empty nodes considered in one iteration as candidates for scale down with drain.<br>Lower value means better CA responsiveness but possible slower scale down latency.<br>Higer value can affect CA performance with big cluster (hundreds of nodes).<br>Set to non-positive value to turn this heuristic off - CA will not limit the number of nodes it considers. | 30 |
| `scale-down-candidates-pool-ratio` | A ratio of nodes that are considered as additional non-empty candidates for scale down, when some candidates from previous iteration are no longer valid.<br>Lower value means better CA responsiveness, but possible slower scale down latency.<br>Higher value can affect CA performance with big clusters (hundreds of nodes).<br>Set to 1.0 to turn this heuristics off - CA will take all nodes as additional candidates. | 0.1 |
| `scale-down-candidates-pool-min-count` | Minimum number of nodes that are considered as additional non-empty candidates for scale down when some candidates from previous iteration are no longer valid.<br>When calculating the pool size for additional candidates, we take<br>`max(#nodes * scale-down-candidates-pool-ratio, scale-down-candidates-pool-min-count)`. | 50 |
| `scan-interval` | How often cluster is re-evaluated for scale up or down. | 10 seconds |
| `max-node-provision-time` | Maximum time CA waits for node to be provisioned. | 15 minutes |
| `expander` | Type of node group expander to be used in scale up.  | random |
| `write-status-configmap` | Should CA write status information to a `configmap`.  | true |
| `balance-similar-node-groups` | Detect similar node groups and balance the number of nodes between them. | false |
| `leader-elect` | Start a leader election client and gain leadership before executing the main loop.<br>Enable this when running replicated components for high availability. | true |

Please reference Cluster AutoScaler for more detailed informations.
https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-are-the-parameters-to-ca

## Apply Configuration
You can apply configuration by the following command.
```bash
$ podder cluster apply
```
> You will be asked whether to apply your configuration after `podder cluster apply` command. Input yes if you apply, else input no.


Environament is automatically built according to your configluration on `.podder/cluster.yml`.


## Destroy Environment
You can destroy configuration by the following command.
```bash
$ podder cluster destroy
```
