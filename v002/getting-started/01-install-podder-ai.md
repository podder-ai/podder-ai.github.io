# Install Podder.ai

## Before you install
You need to get credential from Podder.ai team. Please contact us first.

## Download Podder Installer
You can download podder-installer according to your machine from the following URL.
#### 0.2.11
- [podder-installer_linux_amd64](https://s3-ap-northeast-1.amazonaws.com/podder-downloads/podder-installer/0.2.11/podder-installer_linux_amd64)


## Execute Podder Installer
You can execute podder-installer by the following command. (eq:  Linux AMD64)
```bash
# Add permission to execute podder installer
$ chmod 755 podder-installer_linux_amd64
# Execute podder-installer
$ sudo ./podder-installer_linux_amd64
```

Now, you will be asked several questions.
```bash
# You can input the credential from Podder.ai team
$ Please input your access key: ******
# You can input "y" to install
$ Docker is not installed. Do you want to continue to install Docker? [y/n] y
$ kubectl is not installed. Do you want to continue to install kubectl? [y/n] y
$ Helm is not installed. Do you want to continue to install Helm? [y/n] y
$ AWS CLI is not installed. Do you want to continue to install AWS CLI? [y/n] y
$ Ansible is not installed. Do you want to continue to install Ansible? [y/n] y
```

After installation is complated, you can check if podder is installed by the following command.
```bash
$ podder --version
Authorizing podder-cli utilization ...
podder, version 0.2.11
```



