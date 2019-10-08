# Deploy Your First Application
## What You Learn
Let's deploy your first application using the application your built in the previous section. Here you can learn the following:
- How to build a cluster using Podder CLI
- How to deploy application using Podder CLI
> In this tutorial, we will create cluster on AWS

## Configure Environment
### Preparation
You need to prepare the following credentials:
- Container Registry (DockerHub, ECR)
- AWS
  - EKS
> Note that you will be charged money for utilization of AWS.

### Initialize cluster setting
To begin with, you will initialize cluster setting by using the following command.
```bash
$ podder cluster init
```

You will be asked your environment setting configuration, such as provider and cluster settings. Please input value according to your environment needs.
```bash
$ podder cluster init
Authorizing podder-cli utilization ...
Cluster name [podder]: podder
Cluster Provider (eks) [eks]: eks
Docker registry type (docker_hub, self_hosted, ecr) [docker_hub]: docker_hub
Setting up provider
AWS access key id: 
AWS secret access key: 
AWS Region [ap-southeast-1]: ap-southeast-1
Setting up cluster
CIDR block of VPC [10.0.0.0/16]: 10.0.0.0/16
Auto scaling worker group number [1]: 1
Please configure auto scaling worker group [1]
[1] Name [worker-group-1]: worker-group-1]
[1] Min node number [1]: 1
[1] Max node number [1]: 3
[1] Instance type [a1.2xlarge]: t3.large
[1] Storage size (GB) [50]: 50
Setting up database
Datatbase type (default, rds_aurora_mysql) [default]: default
Setting up persistent volumes
Please provide your Docker Hub information
Docker Hub username [username]: xxxxxx
Docker Hub email [email@sample.com]: xxxxxx@xxxxxx.com
Docker Hub password [********]: 
Docker Hub image namespace [xxxxxx]: xxxxxx
You can edit the file /.podder/cluster.yml, then run `podder cluster apply`
```

### Apply configuration
You can apply this configuration by the following command.
```bash
$ podder cluster apply --yes
```
> It takes more than 10 minutes to create cluster on AWS.

After completed, you can see a cluster on AWS console.

## Deploy Application
In this section, you will deploy your application created in `Run Your First Application` tutorial. (If you have not done yet, please finish the tutorial first.)

### Before Starting Deployment
You need to export the following variables to control AWS EKS from your terminal.
```bash
$ export AWS_PROFILE=podder-user
$ export KUBECONFIG=YOUR_PROJECT_DIRECTORY_ABSOLUTE_PATH/.podder/cluster/kube_config
```

### Initialize deployment setting
To begin with, you will initialize deploy setting by using the following command.
```bash
$ podder deploy init
```

You can see `deploy.yml` is created under config directory.

### Configure deployment
If you would like to configure deployment settings, you can edit `deploy.yml`. In this tutorial, we use default deployment setting. (Please check `Deployment` section in Guides for more information)

### Build and Push application
After you configure deployment settings, you need to build and push docker image of each tasks and pipeline into your Docker Hub repository. You can do it by the following command.
```bash
$ podder deploy push
```

### Deploy application
Now, you can deploy your application into your cluster by the following command.
```bash
$ podder deploy apply --yes
```

### Start application
Finally, you can start with your application by the following command.
```bash
$ podder app start
```

After it's completed, you can get URL for Airflow dashboard.
```
$ export HOSTNAME=$(kubectl get svc -n default podder-pipeline-service -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
$ echo http://$HOSTNAME
```

You can access to Airflow dashboard from the URL.


## Delete Cluster
After you finish the tutorial, do not forget to delete the cluster. Otherwise, you will be charged a lot.
You can delete cluster by the following command.
```bash
$ podder cluster destroy --yes
```