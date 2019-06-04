# Deployment
Podder.ai enables you to deploy your AI application to the cluster easily. We will explain the basic deployment commands on Podder.ai and how to modify configuration to match your needs.
> Before you deploy, you need to setup your `cluster.yml`. Please check `Environment Configuration` Section.

## Initialize Configuration 
You can initialize configuration by the following command.
```bash
$ podder deploy init
```

`deploy.yml` is created under config directory. You can define deployment pipeline and tasks here. 
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