# The purpose of this repo is to learn how to use Dynamic Expressions in Terraform configuration language.

This repo is based on [HashiCorp learning guide](https://learn.hashicorp.com/tutorials/terraform/expressions?in=terraform/configuration-language).

The configuration is creating EC2 instances running a pre-built webapp behind Elastic Load Balancer(ELB) in AWS.

- Used [`locals`](https://www.terraform.io/language/values/locals) to assign expressions to variables for reuse.
- Used [`conditionals`](https://www.terraform.io/language/expressions/conditionals) to declare if the configuration needs to contain multiple instances for high-availability.
- Used the [`splat`](https://www.terraform.io/language/expressions/splat) expression to return multiple private IPs in the network.

##### Name expressions with `locals`

`Locals` assign a name to an expression like a variable assigns a name to a value. Assigning `locals` means you can reuse an expression multiple times within a configuration without repeating it.

In this configuration the `locals` block is used to assign a complex expression of a prefix name and specific tags to the resources.

##### Conditional expression

Conditional expressions determine the action of a configuration based on true or false values.
The `local` block accepts conditional criteria. If the `name` variable is blank, the output of the `random_id` resource is the tag value.
In this configuration the syntax of a conditional expression first names the condition. if `var.name` is not (`!=`) empty, assign the `var.name` value; else, assign the new `random_id` resource as the name value.

##### Conditional count criteria

In the `variables.tf` the boolean variable `high_availability` is the conditional criteria for the instance count.
In the `main.tf` file in the resource `aws_instance` if the `count` is 1 the `associate_public_ip_address` is true; else the `associate_public_ip_address` is false.

##### Return multiple values with the `splat` expression

The `splat` expression captures all objects in a list that share an attribute. The special `*` symbol iterates over all of the elements of a given list and returns information based on the shared attribute which is define.

In the `outputs.tf` the `private_addresses` output will return the private DNS of all instances created by the `aws_instance.ubuntu` resource.

### Prerequisites to run the configuration

Some AWS knowledge (ec2 instance, security groups)

- The [Terraform CLI](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- AWS Credentials [configured for use with Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#authentication)
- The [git CLI](https://git-scm.com/downloads)

**Note**:Some of the infrastructure in this configuration may not qualify for the AWS free tier. Destroy the infrastructure at the end of the guide to avoid unnecessary charges.

### Create and destroy the infrastructure

Clone the repository:
```
git clone https://github.com/krasteki/terraform-expressions.git
```

Navigate to the repo directory:
```
cd terraform-expressions
```

Initialize the configuration:
```
terraform init
```

Apply the configuration:
```
terraform apply
```

Clean up resources to avoid additional charges:
```
terraform destroy
```
