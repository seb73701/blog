---
title: Building a Kubernetes Cluster on AWS
description: 
published: true
date: 2023-06-20T20:23:54.161Z
tags: 
editor: markdown
dateCreated: 2023-06-17T10:06:17.991Z
---

# Building a Kubernetes Cluster on AWS

The excellent [Kubernetes](https://github.com/kubernetes/kubernetes) [documentation](http://kubernetes.io/docs/) includes a [guide](http://kubernetes.io/docs/getting-started-guides/aws/) that covers how to build and run a Kubernetes cluster on AWS with the kube-up script. However, when it comes to customizing that install, the details are a little sparse. In this post, I am going to go over just one way you can customize the cluster. Hopefully, this will provide a little more transparency about what is going on under the hood and give you a little more control over how your cluster is built.

## The Default Bootstrap Script
The [AWS guide](http://kubernetes.io/docs/getting-started-guides/aws/) instructs you to run a command like this:

```shell
export KUBERNETES_PROVIDER=aws; curl -sS https://get.k8s.io | bash
```

This downloads a bash script that is executed with the KUBERNETES_PROVIDER variable set to aws. It then executes that script and the cluster installation proceeds. For the most part, what the bootstrap script does is download the latest Kubernetes release, extract it, and then run the ./cluster/kube-up.sh script that is distributed with it. We are going to break this up into a few steps so that we can choose the version of the release we want to install and customize some of the AWS settings like what region the cluster is built in, the size of the instances, and how many nodes to create.

## Building a Custom Kubernetes Cluster on AWS
In the following examples, assume that I am executing all of these commands from an empty directory called `~/Projects/k8s/aws`.

1. Download the release you are interested in. I am using version 1.1.8 in this example:

```shell
K8S_VERSION=v1.1.8; curl -SLO https://github.com/kubernetes/kubernetes/releases/download/$K8S_VERSION/kubernetes.tar.gz
```

2. Extract it.

```shell
tar xzvf kubernetes.tar.gz
```

At the end of this step, you should have a directory structure that looks something like `~/Projects/k8s/aws/kubernetes`.

You can delete the `kubernetes.tar.gz` file at this point if you want to.

3. Create an install script in your project directory.

We are going to create a very simple shell script that will include our AWS customizations and then run the standard `kube-up.sh` script. Here is an example (which I named `bootstrap-cluster.sh`):

```shell
export KUBERNETES_PROVIDER=aws
export KUBE_AWS_ZONE=us-east-1d
export MASTER_SIZE=m3.medium
export NODE_SIZE=t2.small
export NUM_NODES=5
export AWS_SSH_KEY=$HOME/.ssh/id_rsa
   
function cluster_up {
  ./kubernetes/cluster/kube-up.sh
}
    
function cluster_down {
  ./kubernetes/cluster/kube-down.sh
}
    
if [[ "${1:-up}" == "down" ]]; then
  cluster_down
else
  cluster_up
fi
```

Let’s quickly review the settings here.

- `KUBERNETES_PROVIDER` : `aws`. This tells kube-up.sh to use the AWS variants of the install scripts.
- `KUBE_AWS_ZONE`: `us-east-1d`. I want my cluster built in the us-east-1 region (and us-east-1d availability zone) instead of the default us-west-2a.
- `MASTER_SIZE`: `m3.medium`. Use the [m3.medium](https://aws.amazon.com/ec2/instance-types/#M3) size for the Kubernetes master instance.
- `NODE_SIZE`: `t2.small`. Use the [t2.small](https://aws.amazon.com/ec2/instance-types/#t2) size for each of the Kubernetes node instances.
- `NUM_NODES`: `5`. I want to run 5 nodes (instead of the default 4).
- `AWS_SSH_KEY`: `$HOME/.ssh/id_rsa`. I want to use my existing SSH key to access the cluster instances.

By default, the Kubernetes bootstrap script does some calculations to determine the node sizes to use. In this case, m3.medium would have been the default for the master size (it bumps it up to m3.large if you run more than 5 nodes) so I am just being explicit here. The script defaults the node size to t2.micro unless you are running 50 or more nodes. For my test cluster, I want just a little more memory to work with so I am setting NODE_SIZE to t2.small.

Of course, feel free to change any of the above variables to values that are applicable to you. There are also many other options you can configure, if you want. Check out the [`config-default.sh`](https://github.com/kubernetes/kubernetes/blob/release-1.2/cluster/aws/config-default.sh) script for the details.

4. Build the cluster

```shell
./bootstrap-cluster.sh
... Starting cluster using provider: aws
... calling verify-prereqs
... calling kube-up
Starting cluster using os distro: vivid
Uploading to Amazon S3
...
```

After several minutes, you should have a customized, working Kubernetes cluster running on AWS.

## Destroying your Cluster
To tear down your cluster, you can run the same script with down as an argument.

```shell
./bootstrap-cluster.sh down
```

Kubernetes makes it pretty easy to get started on various cloud providers, including AWS. However, you’re certainly going to want to be able to customize your environments and that is going to require a bit more effort. I hope this article can help with your AWS-based Kubernetes deployments. More advanced deployments can be done with tools like [CloudFormation](https://aws.amazon.com/cloudformation/) or (better yet) [Terraform](https://www.terraform.io/).

Source : https://ryaneschinger.com/