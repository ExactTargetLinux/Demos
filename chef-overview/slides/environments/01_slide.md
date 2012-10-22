# Environments
## Overview

Environments in Chef provide a mechanism for managing different environments
such as production, staging, development, and testing, etc with one Chef
setup.

In our infrastructure, we have an environment for each stack.  For SocialEngage,
we have an environment for dev, qa, staging, and production.

With environments, you can specify per environment run lists in roles, per
environment cookbook versions, and environment attributes.

# Environments
## Cookbook Versioning

We version our cookbooks with the Chef Environments. By doing this, we load a
specific cookbook version no matter what the latest and greatest is.  This is
done to promote consistancy and standardization in each Chef Environment.

If a cookbook version is not specified in the Chef Environment, then the latest
version of the cookbook is loaded during the run of chef-client.

In the Cookbook Metadata, a cookbook version attribute exists.  As I mentioned
earlier, this version should be bumped when editing a cookbook.  This is why...

Once a cookbook has been modified, version bumped, and pushed up to the Chef
Server, You would then go into the particular environment file you want to test
this modification in, and bumpt the cookbook version definition in that environment,
push that environment up to the Chef Server, then run chef-client on the
particular nodes involved.

# Environments
## Attributes

The Chef Environment is a great place to put attributes about an environment. These
Attributes can be used in recipes to load a particular variable about an environment.

For example, we have a different graphite server for each stack.  So in each
Chef Environment, we load a different VIP for graphite.

# Environments
## Override Attributes

In the cookbook that configures graphite, we have a default attribute to
instruct the machine to not write metrics to graphite by default for chef-client.

This is due to the fact that graphite does not exist in every stack.

To override this, we specify an override attribute to instruct the machine to
_write_ metrics to graphite with an attribute that overrides the default one
defined by the cookbook.

# Environments
## Package Version Pinning

In the environment, We can specify a specific package version for package installation.

For example, in the _xtins1_ Chef Environment, for hadoop, we specify the base package
version as `0.20.2+923.97-1~lucid-cdh3`.  This will pin that particular package version
when it is installed by chef-client.

The recipe that installs the package needs to gather this information from the
environment, and specify during the installation.  This is an argument in the recipe
itself.

