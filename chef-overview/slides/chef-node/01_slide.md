# Chef::Node

The first class citizen in Chef is the `Chef::Node`. The server stores
a copy of the node, and indexes it for search.

Ultimately, the node itself is the authority for its own
configuration. The `Chef::Node` object contains the run list and
attributes about the node itself.

An instance of `Chef::Node` is created for the Chef run, available in
recipes, templates and other cookbook components as "`node`".

# Chef::Node
## Node Name

Every node has a name which must be unique. By default, Chef will use
the node's fully-qualified domain name, since that is fairly safe.

# Chef::Node
## Node Attributes

Node's have attributes, which are data about the node
itself. Attributes can come from a variety of sources in Chef. Most
commonly:

* Automatic (ohai)
* Cookbooks (attributes or recipes)
* Roles

Other locations are available, but they are out of scope for this course.

# Chef::Node
## Run List

The run list is an array of roles and/or recipes to apply on the node.

The expanded list of roles are stored in the attribute
`node["roles"]`.

The expanded list of recipes are stored in the attribute
`node["recipes"]`.

Recipes that are included via `include_recipe` method are *not* expanded.

# Chef::Node
## Run List (Continued)

The run list is an array, which means it is ordered by numeric index
of the elements it contains.

The expanded run list's recipes are in the order they appear in run
list elements (node itself or in roles).

# Chef::Node
## Additional Resources

* http://wiki.opscode.com/display/chef/Nodes
* http://wiki.opscode.com/display/chef/Attributes
* http://wiki.opscode.com/display/chef/Environments
