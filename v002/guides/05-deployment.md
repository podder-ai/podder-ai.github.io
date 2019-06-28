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
  aws_access_key: YOUR_AWS_ACCESS_KEY
  aws_secret_access_key: YOUR_AWS_SECRET_ACCESS_KEY
  poll_period: 5s
  scale_down_cool_down: 10s
  scale_up_cool_down: 10s
  scale_up_messages: 100            # Scale up message size
  scale_down_messages: 20           # Scale down message size
  max_pods: 3                       # Max pods size
  min_pods: 1                       # Min pods size
```

### Vertical Pod AutoScaler
Podder cluster use Vertical Pod AutoScaler (VPA) to automatically set up-to-date resource requests for the containers in their pods. When configured, it will set the requests automatically based on usage and thus allow proper scheduling onto nodes, so that appropriate resource amount is available for each pod. It can both down-scale pods that are over-requesting resources, and also up-scale pods that are under-requesting resources based on their usage over time.

Document: https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler

#### How to configure VPA on Podder cluster
Podder CLI use `config/deploy.yml` for configure VPA for Podder-pipeline and Podder-task. Please change your settings in `config/deploy.yml` before running `podder deploy apply`.

```
# Vertical pod auto scaling setting for podder-task
# enabled: ("true"/"false")
# maxAllowed: (limitation of vertical po auto scaling
#  cpu: 80m
#  memory: 2Gi # Gi for GB, Mi for MB
podder_task_vertical_pod_autoscaling:
  enabled: false

# Vertical pod auto scaling setting for podder-pipeline
# enabled: ("true"/"false")
# maxAllowed: (limitation of vertical po auto scaling
#  cpu: 80m
#  memory: 2Gi # Gi for GB, Mi for MB
podder_pipeline_vertical_pod_autoscaling:
  enabled: false
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
