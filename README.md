# Install Tectonic on AWS with Terraform

Use this guide to manually install a Tectonic cluster on an AWS account. To install Tectonic on AWS with a graphical installer instead, refer to the [AWS graphical installer documentation][aws-gui].

Generally, the AWS platform templates adhere to the standards defined by the project [conventions][conventions] and [generic platform requirements][generic]. This document aims to document the implementation details specific to the AWS platform.

## Getting Started

### Initialize and configure Terraform

#### Get Terraform's AWS modules and providers

Get the modules and providers that Terraform will use to create the cluster resources:

```bash
$ terraform init
Downloading modules...
Get: git::https://github.com/coreos/tectonic-installer.git?ref=1d75718d96c7bdec04d5ffb8a72fa059b1fcb79a
Get: git::https://github.com/coreos/tectonic-installer.git?ref=1d75718d96c7bdec04d5ffb8a72fa059b1fcb79a
Get: git::https://github.com/coreos/tectonic-installer.git?ref=1d75718d96c7bdec04d5ffb8a72fa059b1fcb79a
Get: git::https://github.com/coreos/tectonic-installer.git?ref=1d75718d96c7bdec04d5ffb8a72fa059b1fcb79a
Get: git::https://github.com/coreos/tectonic-installer.git?ref=1d75718d96c7bdec04d5ffb8a72fa059b1fcb79a
Get: git::https://github.com/coreos/tectonic-installer.git?ref=1d75718d96c7bdec04d5ffb8a72fa059b1fcb79a
Get: git::https://github.com/coreos/tectonic-installer.git?ref=1d75718d96c7bdec04d5ffb8a72fa059b1fcb79a
Get: git::https://github.com/coreos/tectonic-installer.git?ref=1d75718d96c7bdec04d5ffb8a72fa059b1fcb79a
Get: git::https://github.com/coreos/tectonic-installer.git?ref=1d75718d96c7bdec04d5ffb8a72fa059b1fcb79a
Get: git::https://github.com/coreos/tectonic-installer.git?ref=1d75718d96c7bdec04d5ffb8a72fa059b1fcb79a

Initializing provider plugins...
- Downloading plugin for provider "template"...
- Downloading plugin for provider "ignition"...
- Downloading plugin for provider "local"...
- Downloading plugin for provider "tls"...
- Downloading plugin for provider "null"...
- Downloading plugin for provider "random"...
- Downloading plugin for provider "archive"...
- Downloading plugin for provider "aws"...
...
```

Next, specify the cluster configuration.

## Customize the deployment

Customizations to the base installation live in `examples/terraform.tfvars`.
Copy the example configuration:

```bash
$ cp examples/terraform.tfvars terraform.tfvars
```

Edit the parameters with your AWS details, domain name, license, etc. [View all of the AWS specific options and the common Tectonic variables][vars].

## Deploy the cluster

Test out the plan before deploying everything:

```bash
$ terraform plan
```

Next, deploy the cluster:

```bash
$ terraform apply
```

This should run for a little bit, and when complete, your Tectonic cluster should be ready.

## Access the cluster

The Tectonic Console should be up and running after the containers have downloaded. You can access it at the DNS name configured in your variables file.

Inside of the `generated/` folder you should find any credentials, including the CA if generated, and a `kubeconfig`. You can use this to control the cluster with `kubectl`:

```bash
$ export KUBECONFIG=generated/auth/kubeconfig
$ kubectl cluster-info
```

## Work with the cluster

For more information on working with installed clusters, see [Scaling Tectonic AWS clusters][scale-aws], and [Uninstalling Tectonic][uninstall].

## Known issues and workarounds

See the [troubleshooting][troubleshooting] document for workarounds for bugs that are being tracked.


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
