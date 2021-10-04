# ORO Commerce Terraform Cloud Infrastructure 

![ORO Cloud Terraform Infrastructure](https://user-images.githubusercontent.com/9213670/135354428-cbcbd29a-65ea-4209-85d5-c0937fa51864.png)

OroCommerce B2B and B2C Cloud has the next components:
 - Web Node with Auto Scaling and support of ARM Graviton 2 instances,
 - ElasticSearch, 
 - Relational database PostgreSQL RDS Aurora
 - Redis ElasticCahe for Sessions and Caches 
 - CloudWatch for monitoring and allerting 
 - AWS SES(Simple Email Service)/SMTP to send email 
 - NFS/EFS shared network file system 

## Quick start

1. [Install Terraform 0.15 or newer](https://learn.hashicorp.com/tutorials/terraform/install-cli)
1. [Install Terragrunt 0.29 or newer](https://terragrunt.gruntwork.io/docs/getting-started/install/)
1. Optionally, [install pre-commit hooks](https://pre-commit.com/#install) to keep Terraform formatting and documentation up-to-date.

If you are using macOS you can install all dependencies using [Homebrew](https://brew.sh/):
```
    $ brew install terraform terragrunt pre-commit
```
## Configure access to AWS account

The recommended way to configure access credentials to AWS account is using environment variables:

```
$ export AWS_DEFAULT_REGION=ap-southeast-1
$ export AWS_ACCESS_KEY_ID=...
$ export AWS_SECRET_ACCESS_KEY=...
```

Alternatively, you can edit `terragrunt.hcl` and use another authentication mechanism as described in [AWS provider documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#authentication).

## Create and manage your infrastructure

Infrastructure consists of multiple layers (oro_auto_scaling, postgresql, load_balancer, ...) where each layer is described using one [Terraform module](https://www.terraform.io/docs/configuration/modules.html) with `inputs` arguments specified in `terragrunt.hcl` in respective layer's directory.

Navigate through layers to review and customize values inside `inputs` block.

There are two ways to manage infrastructure (slower&complete, or faster&granular):
- **Region as a whole (slower&complete).** Run this command to create infrastructure in all layers in a single region:

```
$ cd region
$ terragrunt run-all apply
```

- **As a single layer (faster&granular).** Run this command to create infrastructure in a single layer (eg, `oro_auto_scaling`):

```
$ cd ap-southeast-1/ro_auto_scaling
$ terragrunt apply
```

After the confirmation your infrastructure should be created.

## Destroy/Delete infrastructure

**destroy-all** (DEPRECATED: use run-all)
DEPRECATED: Use **run-all destroy** instead.

```
 terragrunt run-all destroy
```

Destroy a ‘stack’ by running ‘terragrunt destroy’ in each subfolder.

## Provisioned infrastructure configuration values
After **apply** command run:
```
terragrunt run-all output
#or
terragrunt output-all
```
or by modules:
```
cd load_balancer
terragrunt run-all output
#or
terragrunt output-all --terragrunt-non-interactive
```

The terraform/trragrunt **output** command is used to extract the value of an output variable from the state file.

With no additional arguments, output will display all the outputs for the root module. If an output NAME is specified, only the value of that output is printed.

The command-line flags are all optional. The list of available flags are:

**-json** - If specified, the outputs are formatted as a JSON object, with a key per output. If NAME is specified, only the output specified will be returned. This can be piped into tools such as jq for further processing.
**-raw** - If specified, Terraform will convert the specified output value to a string and print that string directly to the output, without any special formatting. This can be convenient when working with shell scripts, but it only supports string, number, and boolean values. Use -json instead for processing complex data types.
**-no-color** - If specified, output won't contain any color.
**-state=path** - Path to the state file. Defaults to "terraform.tfstate". Ignored when remote state is used.

## References

* [Terraform documentation](https://www.terraform.io/docs/) and [Terragrunt documentation](https://terragrunt.gruntwork.io/docs/) for all available commands and features
* [Terraform AWS modules](https://github.com/terraform-aws-modules/)
* [Terraform modules registry](https://registry.terraform.io/)
* [Terraform best practices](https://www.terraform-best-practices.com/)


