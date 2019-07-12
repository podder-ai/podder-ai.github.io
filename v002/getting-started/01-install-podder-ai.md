# Install Podder.ai

## Before you install
You can install Podder.ai by using `podder-installer`. To execute it, you need to get credential from Podder.ai team. Please contact us first.

Also, you need the following software installed in your environment.
- pip
- Python (v3.6 or higher is required)

When you execute podder-installer, the following softwares are installed.
- curl
- unzip
- Docker
- docker-compose
- kubectl
- Helm
- AWS CLI
- aws-i-am-authenticator
- Terraform
> The newest version at current date is installed

## Download Podder Installer
You can download podder-installer according to your machine from the following URL.

#### Version 1.1.0

- [Linux 64bit](https://podder-downloads.s3-ap-northeast-1.amazonaws.com/podder-installer/1.1.0/podder-installer-1.1.0_linux_amd64)

#### Version 1.0.5

- [Linux 64bit](https://podder-downloads.s3-ap-northeast-1.amazonaws.com/podder-installer/1.0.5/podder-installer-1.0.5_linux_amd64)

## Execute Podder Installer
You can execute podder-installer by the following command. (eq:  Linux AMD64)
```bash
# Add permission to execute podder installer
$ chmod 755 podder-installer-1.1.0_linux_amd64

# Execute podder-installer
$ sudo ./podder-installer-1.1.0_linux_amd64
```

Now, you will be asked several questions.
```bash
# Please input the credential from Podder.ai team
$ Please input your access key: ******

# Please input "y" to install
$ docker is not installed. Do you want to continue to install docker? [y/n] y
$ docker-compose is not installed. Do you want to continue to install docker-compose? [y/n] y
$ kubectl is not installed. Do you want to continue to install kubectl? [y/n] y
$ Helm is not installed. Do you want to continue to install Helm? [y/n] y
$ AWS CLI is not installed. Do you want to continue to install AWS CLI? [y/n] y
$ terraform is not installed. Do you want to continue to install terraform? [y/n] y
$ aws-iam-authenticator is not installed. Do you want to continue to install aws-iam-authenticator? [y/n] y
```

After installation is completed, you can check if podder is installed by the following command.
```bash
$ podder --version
Authorizing podder-cli utilization ...
podder, version 1.1.0
```

## Recommended System Requirements
To ensure Podder.ai runs successfully, please make sure your environment satisfy the following requirements.

### Requirements for Podder.ai
#### Supported OS
- Linux
- Ubuntu

#### Memory
- 8GB

#### CPU
- 2

#### Storage
- 200GB

### Requirements for Kubernetes
#### Supported OS for Kubernetes
- Ubuntu 16.0 Debian 9
- CentOS 7
- RHEL 7
- Fedora 25/26 (best-effort)
- HypriotOS v1.0.1+
- Container Linux (tested with 1576.4.0) 4+

#### Supported OS for Kubernetes Node
Currently, Podder.ai only provisions via cloud provider.
For EKS, all types of OS are supported.

#### Memory
- 2GB

#### CPU
- 2

#### Network
- Complete network across every machine within a cluster.
- Unique host name, Mac address, product UUID.

#### Disk
- Disable Swap
