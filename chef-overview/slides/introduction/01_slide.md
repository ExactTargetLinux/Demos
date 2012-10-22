# Introduction
## A High Level Overview of Chef
<center><img src="../images/oc-chef-logo.png" width="500"></img></center>

<div class="titlePage">A High Level Overview of Chef in the ExactTarget Infrastructure</div>

# Introduction
## Objectives

* Understand the following:
  * Chef's tools and architecture
  * Cookbooks and Recipes
  * Environments and Roles
  * Data Bags
  * Cookbook and Package Version Pinning
  * The ExactTarget Chef Infrastructure

# Introduction
## Agenda

* Introduction
* Chef Architecture
* Anatomy of a Chef Run
  * Building the Node
  * Node Objects
  * Node Run List
  * Node Convergence
  * Report and Exception Handlers
  * Additional Resources
* Knife
  * Sub Commands
  * Plugins
* Cookbooks and Recipes
* Nodes
* Roles
* Environments
* Data Bags

# Introduction
## 10,000 Foot View

At a High Level, Chef is:

* A library for configuration management
* A systems management platform
* An API for the entire infrastructure

With Chef, you write abstract definitions as source code to describe how you want each part of your infrastructure to be built, and then apply those descriptions to individual servers.

The result is a fully automated infrastructure: when a new server comes on line, the only thing you have to do is tell Chef what role it should play in your architecture.
