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
```

After you input the all information `cluster.yml` is created under .podder directory.

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