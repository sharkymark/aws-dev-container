# AWS Development Container

A development container with AWS tools and Kubernetes utilities pre-installed for cloud development workflows.

This is alternative to installing the CLIs on your local machine. This container is designed to be used with Visual Studio Code Remote - Containers extension, which allows you to develop inside a container.

## Features

- AWS CLI v2
- eksctl (EKS CLI)
- kubectl (Kubernetes CLI)
- Helm
- Git configuration
- VS Code extensions

## Environment Variables Required

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_DEFAULT_REGION`
- `GITHUB_TOKEN`
- `GITHUB_USER_NAME`
- `GITHUB_USER_EMAIL`

## Included Tools

- AWS CLI
- AWS eksctl
- Kubectl
- Helm
- Python 3 and pip
- Git
- cURL

## Pre-installed VS Code Extensions

- Python
- Pylance
- GitHub Copilot
- Claude Developer
- Continue

## Validate the AWS environment variables
To validate the AWS environment variables, you can use the following command in your terminal:

```bash
aws sts get-caller-identity
```
This command will return the AWS account ID, user ID, and ARN of the IAM user or role associated with the provided credentials. If the command returns an error, it indicates that the AWS environment variables are not set correctly.

Environmental variables can be used in lieu of AWS credentials file. The AWS CLI will automatically use the environment variables if they are set. If you want to use the AWS credentials file, you can run:

```bash
aws configure
```
This command will prompt you to enter your AWS access key, secret key, region, and output format. The credentials will be stored in the `~/.aws/credentials` file. This directory is in `.gitignore` to prevent it from being committed to the repository.

### Sample AWS commands

```bash
aws iam list-users
aws s3 ls
aws eks list-clusters
aws ec2 describe-regions --query "Regions[].{RegionName:RegionName}" --output table
aws ec2 describe-availability-zones --region us-east-1 --query "AvailabilityZones[].{ZoneName:ZoneName}" --output table
aws ec2 describe-instance-types --query "InstanceTypes[].{InstanceType:InstanceType}" --output table
aws ec2 describe-instance-types --filters "Name=instance-type,Values=*.micro" --query "InstanceTypes[].{InstanceType:InstanceType, vCPUs:VCpuInfo.DefaultVCpus, Memory:MemoryInfo.SizeInMiB}" --output table
```

### Validate eksctl CLI with AWS environment variables
To validate eksctl CLI with the AWS environment variables, you can use the following command in your terminal:

```bash
eksctl get cluster
```
This command will return a list of EKS clusters in the specified region. If the command returns an error, it indicates that the eksctl environment variables are not set correctly.


## Setup Kubectl

To set up `kubectl`, you need to install the AWS CLI and configure it to use your AWS credentials. You can do this by running the following command:

```bash
aws eks update-kubeconfig --name <cluster-name>
```
This command will update your kubeconfig file with the necessary credentials to access your EKS cluster. Replace `<cluster-name>` with the name of your EKS cluster. The kubeconfig file is usually located at `~/.kube/config` and is also included in `.gitignore` to prevent it from being committed to the repository.

## Validate Helm

To validate Helm, you can use the following command in your terminal:

```bash
helm repo add nginx-stable https://helm.nginx.com/stable
helm repo update
helm search repo nginx-stable
```

## Validate GitHub environment variables
To validate the GitHub environment variables, you can use the following command in your terminal:

```bash
curl -H "Authorization: token ${GITHUB_TOKEN}" https://api.github.com/user
```
This command will return the user information associated with the provided token. If the command returns an error, it indicates that the GitHub environment variables are not set correctly.

## AWS Free Tier Details

More about the AWS Free Tier can be found at [AWS Free Tier](https://aws.amazon.com/free/).

### As of 2025-04-16:

The AWS Free Tier is a program that allows you to use certain AWS services for free, up to specified limits. The free tier includes:
- 750 hours of Amazon EC2 t2.micro or t3.micro instances each month for one year.
- 5 GB of Amazon S3 storage.
- 1 million requests per month for AWS Lambda.
- 15 GB of data processing and 750 hours of load balancing per month for Amazon Elastic Load Balancing.
- EC2 instance limits are t2.micro or t3.micro instances
- EKS is not included in the free tier, but you can use it with the free tier EC2 instances
- EKS control plane is $0.10 per hour

## Named Volumes
The following named volumes are configured and managed by Docker in the `devcontainer.json` file:
- `aws-cli-config`: This volume is used to store the AWS CLI configuration files. It is mounted at `/root/.aws` in the container. This is in case you want to use the AWS credentials file instead of environment variables.
- `kubectl-config`: This volume is used to store the kubectl configuration files. It is mounted at `/root/.kube` in the container.
- `.config`: This volume is used to store the configuration files. e.g., helm It is mounted at `/root/.config` in the container. 
- `.cache`: This volume is used to store the cache files. e.g., helm It is mounted at `/root/.cache` in the container.

## Local machine alternative

If you prefer to install the AWS CLI and other tools on your local machine instead of using a container, installing the following with `brew`:

```bash
brew install awscli eksctl kubectl helm
```


## License

MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.