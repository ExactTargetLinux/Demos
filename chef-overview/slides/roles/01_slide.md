# Roles
## Overview

A role provides a means of grouping similar features of similar nodes,
providing a mechanism for easily composing sets of functionality. At
scale, you almost never have just one of something, so you use roles
to express the parts of the configuration that are shared by a group
of Nodes.

Roles consist of the same parts as a node: Attributes and a Run List.

Nodes can have multiple roles applied, and they will be expanded in
place, providing for a complete recipe list for that node.

When the Chef client runs, it merges its own attributes and run list
with those of any roles it has been assigned.

# Roles
## Components of a Role

Run list

* recipes
* roles

Attributes applied to the nodes that have this role.

* default
* override

# Roles
## Creating Roles

The `roles` directory in the chef-repo contains the roles for the infrastructure.

Write roles using a Ruby DSL.

    $EDITOR roles/base.rb

# Roles
## Uploading the role to the Chef Server

Upload the roles to the Chef Server. Knife will automatically look for
the specified file in the `roles` directory.

    knife role from file base.rb

Roles are converted to JSON on the server.

# Roles
## Ruby or JSON

Chef supports Ruby DSL or JSON for role files in chef-repo.

* Ruby DSL roles require less syntax.
* Ruby is converted to JSON by Knife when uploading.
* Chef Server stores roles as JSON.
* `knife role show` displays JSON and can be redirected to a file.

# Roles
## Building Roles

What kind of roles do we need?

* `base` role.
* per-service roles.
* platform roles.

# Roles
## Base Role

We use an OS Based role that gets applied to every system based on OS in the infrastructure.

The exception to this is _SocialEngage_. For these servers, we use `cotweet_base`

These roles contain the basics that all systems should have based on the ExactTarget Infrastructure.

# Roles
## roles/base_role.rb

# Roles
## Per-service Roles

In a service oriented architecture, each different service should have
its own role with the recipes that determine how to fulfill that
service.

For example, it is common in a web-application architecture to have
webservers.

In this example, we would provide a run list that is composed of a run_list,
maybe `recipe "apache2", recipe "monit:apache2", role[nagios::client]`.

We may also include some default attributes, such as:
specifying the the default ports apache2 should listen on.

You could also specify this as an override attribute.

# Apply Roles to a Node

Use knife:

    knife node run list add NODE 'role[base]'

or edit the node:

    knife node edit node_name
