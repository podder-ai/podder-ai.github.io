# Deployment
Podder.ai enables you to deploy your AI application to the cluster easily. We will explain the basic deployment commands on Podder.ai and how to modify configuration to match your needs.
> Before you deploy, you need to setup your `cluster.yml`. Please check `Environment Configuration` Section.

## Initialize Configuration 
You can initialize configuration by the following command.
```bash
$ podder deploy init
```

`config/deploy.yml` is created under config directory. You can define deployment pipeline and tasks here. 
```yaml
# deploy.yml
deployments:
- image: docker.io/username/podder-pipeline     # docker image registory
  name: podder-pipeline                         # deployment name
  template: podder-pipeline                     # deployment template (podder-pipeline or podder-task)
- image: docker.io/username/writer-task
  name: writer-task
  template: podder-task
- image: docker.io/username/podder-task
  name: podder-task
  template: podder-task
- image: docker.io/username/sensor-task
  name: sensor-task
  template: podder-task
versions: null
```

## Build and Push Docker Image
You can build and push docker image to your registory by the following command.
```bash
$ podder deploy push
```

## Apply Deployment
You can apply deployment to your cluster by the following command.
```bash
$ podder deploy apply
```

## Delete Deployment
You can delete deployment from your cluster by the following command.
```bash
$ podder deploy delete
```

## Auto Scaling Pods
### AWS SQS 
Let's say you would like to auto scaling pods according to AWS SQS message size. You can add the following configuration to `config/deploy.yml`.
```yaml
deployments:
- image: docker.io/username/podder-pipeline
  name: podder-pipeline
  template: podder-pipeline
- image: docker.io/username/writer-task
  name: writer-task
  template: podder-task
- image: docker.io/username/podder-task
  name: podder-task
  template: podder-task
- image: docker.io/username/sensor-task
  name: sensor-task
  template: podder-task
versions: null
# Add here
pod_autoscaling_settings:
- deployment: podder-pipeline       # Choose from name from deployments
  type: queue
  provider: aws
  queue_url: YOUR_AWS_SQS_URL
  region: YOUR_AWS_SQS_REGION
  poll_period: 5s
  scale_down_cool_down: 10s
  scale_up_cool_down: 10s
  scale_up_messages: 100            # Scale up message size
  scale_down_messages: 20           # Scale down message size
  max_pods: 3                       # Max pods size
  min_pods: 1                       # Min pods size
```

## Assign Pods to Specific Node
Let's say you would like to assign a pod to specific node such as GPU node. You can modify `config/deploy.yml` like below.
```yaml
・・・
node_selectors:
- deployment: podder-pipeline
  node_label: eks-compute-type=cpu
- deployment: sensor-task
  node_label: eks-compute-type=cpu
- deployment: scanner-task
  node_label: eks-compute-type=gpu
- deployment: writer-task
  node_label: eks-compute-type=cpu
```