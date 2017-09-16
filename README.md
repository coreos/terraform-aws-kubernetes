# Install Tectonic on AWS with Terraform

This module deploys a [Tectonic][tectonic] Kubernetes cluster on an AWS account using [Terraform][terraform]. Tectonic is an enterprise-ready distribution of Kubernetes including automatic updates, monitoring and alerting, integration with common authentication regimes, and a graphical console for managing clusters in a web browser.

This module can deploy either a complete Tectonic cluster, requiring a Tectonic license, or a "stock" Kubernetes cluster without Tectonic features.

To install Tectonic on AWS with a graphical installer instead, refer to the [Tectonic graphical installer documentation][aws-gui].

The AWS platform templates adhere to the standards defined by the project [conventions][conventions] and [generic platform requirements][generic]. This document details the specifics of the AWS platform.

## Getting Started

### Customize the deployment

Customizations to the base installation are made to the Terraform variables for each deployment. Examples of the the Tectonic-specific variables are provided in the file `examples/kubernetes.tf`.

Edit the parameters with your AWS details, domain name, and [Tectonic license][register]. To install a basic Kubernetes cluster without Tectonic features, set the `tectonic_vanilla_k8s` key to `true` and omit the Tectonic license.

[View all of the AWS specific options and the common Tectonic variables][vars].

### Initialize and configure Terraform

#### Get Terraform's AWS modules and providers

Get the modules and providers that Terraform will use to create the cluster resources:

```bash
$ terraform init
Downloading modules...
Get: git::https://github.com/coreos/tectonic-installer.git?ref=1d75718d96c7bdec04d5ffb8a72fa059b1fcb79a
Get: git::https://github.com/coreos/tectonic-installer.git?ref=1d75718d96c7bdec04d5ffb8a72fa059b1fcb79a
...

Initializing provider plugins...
- Downloading plugin for provider "template"...
- Downloading plugin for provider "ignition"...
- Downloading plugin for provider "aws"...
...
```

### Deploy the cluster

Test the blueprint before deploying:

```bash
$ terraform plan
```

Next, deploy the cluster:

```bash
$ terraform apply
```

This should run for a short time, and when complete, the cluster should be ready.

### Access the cluster

The Tectonic Console should be up and running after the containers have downloaded. You can access it at the DNS name formed by concatenating the cluster name with the domain configured in the Terraform variables.

Cluster credentials are written beneath the `generated/` directory, including any generated CA certificate and a `kubeconfig` file. You can use this to access the cluster with `kubectl`. This is the only method of access for a Kubernetes cluster installed without Tectonic features:

```bash
$ export KUBECONFIG=generated/auth/kubeconfig
$ kubectl cluster-info
```

## Work with the cluster

For more information on working with installed clusters, see [Scaling Tectonic AWS clusters][scale-aws], and [Uninstalling Tectonic][uninstall].

## Known issues and workarounds

See the [troubleshooting][troubleshooting] document for workarounds and known issues.


[conventions]: https://github.com/coreos/tectonic-docs/blob/master/Documentation/conventions.md
[generic]: https://github.com/coreos/tectonic-docs/blob/master/Documentation/generic-platform.md
[env]: http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-environment
[register]: https://account.coreos.com/signup/summary/tectonic-2016-12
[account]: https://account.coreos.com
[vars]: https://github.com/coreos/terraform-aws-kubernetes/blob/master/variables.md
[troubleshooting]: https://github.com/coreos/tectonic-docs/blob/master/Documentation/troubleshooting/faq.md
[aws-gui]: https://coreos.com/tectonic/docs/latest/install/aws/index.html
[terraform]: https://www.terraform.io/downloads.html
[uninstall]: https://github.com/coreos/tectonic-docs/blob/master/Documentation/install/aws/uninstall.md
[scale-aws]: https://github.com/coreos/tectonic-docs/blob/master/Documentation/admin/aws-scale.md
[release-notes]: https://coreos.com/tectonic/releases/
[verification-key]: https://coreos.com/security/app-signing-key/
[tectonic]: https://coreos.com/tectonic/
