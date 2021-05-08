instance iam roles
your instances will need iam roles to be able to access RDS and make other calls
create those roles here

bonus
as a general rule terraform kinda needs access to everything
however as you have all your work being done in 2 repos each doing separate things
how about setting up a third repo that has 2 roles, each role is locked down
so that that role only has the minimum required access.

Then rather than just have terraform use a role that has access to everything
have the terraform user the ability to assume 2 roles.

Then have terragrunt in each repo assume the role that has the required access.

finally create a new role that allows the creation of other roles and have terragrunt
in this repo assume that role.

Lastly delete that main admin terraform role that you were using.

Now you have a far more secure system :)
