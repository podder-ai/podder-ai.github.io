# Install Podder.ai

## Before you install
You need to get credential from Podder.ai team. Please contact us first.

## Requirements
Before you execute podder-installer, you will need to have following software installed.
- pip
- Python (v3.6 or higher is required)

And when you execute podder-installer, podder-installer will install following software needed, if not already installed.
For the following, newest version at current date will be installed.
- Docker
- Helm
- kubectl
- AWS CLI
- aws-i-am-authenticator
- Terraform

## Download Podder Installer
You can download podder-installer according to your machine from the following URL.

#### Version 1.0.3

- [Linux 64bit](https://podder-downloads.s3-ap-northeast-1.amazonaws.com/podder-installer/1.0.3/podder-installer-1.0.3_linux_amd64)

## Execute Podder Installer
You can execute podder-installer by the following command. (eq:  Linux AMD64)
```bash
# Add permission to execute podder installer
$ chmod 755 podder-installer-1.0.3_linux_amd64

# Execute podder-installer
$ sudo ./podder-installer-1.0.3_linux_amd64
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
podder, version 1.0.3
```

## Recommended System Requirements
To ensure Podder runs successfully, please accomplish following requirements.

### Requirements for Podder
#### Supported OS
- MacOS
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
Currently, only provisioning via cloud provider is supported.

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
