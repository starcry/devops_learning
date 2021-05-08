by the end of this you will be competent in:
* tfenv
* terragrunt
* terraform
* AWS
** at least the basics
** by all means use whatever cloud you wish, I just designed this with AWS in mind

what you will need to install
* tfenv
* terragrunt
* packer
* aws cli

before you start
* setup an aws user account with access id keys for terraform to use
** It will need to have full admin rights
** this isn't the best solution as there is a more optimal one but that's for the bonus round :).

assignment:
* networking
** holds the VPCs, Subnets, Security groups and general networking configurations

* compute
** anything compute related
** RDS & EC2 instances
** Loadbalancers
** S3 can go here too

* iam_other
** is is essentially everything that doesn't fit in the other too
** however iam roles for instances should go in here
** bonus round

terragrunt
terraform has a concept of workspaces, these are used so that the same code can
be deployed to manage multiple environments. each workspace an repo has it's
own state file.

However managing various states can get difficult to handle at scale, this is
where Terragrunt comes in. It can manage workspaces write files and import variables,
You will be doing all of these.

* declare locals in each all.hcl, envs.hcl & unique.hcl
** locals in terragrunt are declared the same way as in terraform
** however you will need to pass these locals into terraform as variables.
** passing locals can be done as below
uniq.hcl
```
locals {
  variable = "hello world"
}
```

<project root directory>/terragrunt.hcl
```
locals {
  unique_vars = read-terragrunt-config("unique.hcl")
  variable    = local.unique_vars.locals.variable
}

inputs = {
  variable = local.variable
}
```

variables.tf
```
variable "variable" {}
```

terragrunt will be executed from the lowest directory so in these assignments
you will be in `envs/dev/team_alpha`, for example. However when calling locals
in files further up the tree you will need to do:
<project root directory>/terragrunt.hcl
```
locals {
  env_vars = read_terragrunt_config(find_in_parent_folders('env.hcl'))
  variable = local.env_vars.locals.variable
```

assignment
* do each assignment laid out in compute, networking and iam_other
* use 2 workspaces, one for team alpha, one for team beta
* statefiles must be stored in S3
* use dynamodb to manage state locking
* each repo needs to have it's own statefile within each workspace
* you will probably need to create some resources from scratch but wherever possible
  use the terraform registry and github
** terraform does the the ability to refer to remote modules
*** module "foo" {
***   source = "<path to git repo/terraform registry>"
*** }
*** if using a git repo use the http git path as the source
*** EG: git::https://github.com/user/repo.git
* use modules
** it's tempting to put everything in main.tf when learning but this is bad practice
** wherever possible create a module and use a module
