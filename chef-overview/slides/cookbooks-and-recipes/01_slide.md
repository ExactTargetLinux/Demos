# Cookbooks and Recipes

<div class="titlePage">Cookbooks and Recipes</div>
<center><img src="../images/cookbook-cover.jpg" /></center>

# Cookbooks and Recipes
## Cookbooks

Cookbooks are the fundimental units of distribution in Chef.  The pull
together all the resources necessary to automate our infrastructure.

At ExactTarget, we store our Cookbooks in a private git repository
hosted on GitHub.com.  Each cookbook version is also stored on the
Chef Server.

# Cookbooks and Recipes
## Cookbook Contents

* Attributes
* Definitions
* File Distribution
* Libraries
* Recipes
* Lightweight Resources and Providers (LWRP)
* Templates
* Metadata

# Cookbooks and Recipes
## Attributes
In the cookbook directory, a sub-directory called `attributes`
contains files with default attributes that are to be given to a
node.  These attributes can be overridden in a role or environment.

It should also be noted that you can specify _default_ attributes in
the environment or roles as well.  This is not recommended since a
new node may not be in a specific environment, and then a particular
recipe will fail to complete due to a nil value.

It is best to define attributes in roles and environments as _override_
attributes, trumping the default specified in the cookbook.


# Cookbooks and Recipes
## Definitions
<font size="14px">Definitions allow you to create reusable collections of one or more
resource.</font>

<font size="14px">Deffinitions are _not_ resources.</font>

<font size="14px">They are replaced by the resources they contain.
You cannot notify a definition to take an action - you can only
notify the resources it creates.</font>

# Cookbooks and Recipes
## Example Definition

    @@@ruby
    define :apache_site, :enable => true do
      include_recipe "apache2"
      if params[:enable]
	execute "a2ensite #{params[:name]}" do
	  command "/usr/sbin/a2ensite #{params[:name]}"
	  notifies :restart, resources(:service => "apache2")
	  not_if do
	    ::File.symlink?("#{node[:apache][:dir]}/sites-enabled/#{params[:name]}") or
	    ::File.symlink?("#{node[:apache][:dir]}/sites-enabled/000-#{params[:name]}")
	  end
	  only_if do ::File.exists?("#{node[:apache][:dir]}/sites-available/#{params[:name]}") end
	end
      else
	execute "a2dissite #{params[:name]}" do
	  command "/usr/sbin/a2dissite #{params[:name]}"
	  notifies :restart, resources(:service => "apache2")
	  only_if do ::File.symlink?("#{node[:apache][:dir]}/sites-enabled/#{params[:name]}") end
	end
      end
    end

# Cookbooks and Recipes
## Example Code for Recipe

    @@@ruby
    # Enable my_site.conf
    apache_site "my_site.conf" do
      enable true
    end
    # Disable my_site.conf
    apache_site "my_site.conf" do
      enable false
    end

# Cookbooks and Recipes
## Resources Created by the Definition

    @@@ruby
    execute "a2ensite my_site.conf" do
      command "/usr/sbin/a2ensite my_site.conf"
      notifies :restart, resources(:service => "apache2")
      not_if do
	::File.symlink?("/etc/apache2/sites-enabled/my_site.conf") or
	::File.symlink?("/etc/apache2/sites-enabled/000-my_site.conf")
      end
    end

# Cookbooks and Recipes
## What this allows us to do...

This allows us to load the cookbook definition in any cookbook, and
utilize the short code in the recipe, in this case `apache_site`, instead
of having a lot of repetitive lines doing the same thing.  This shrinks
the size of our code base, creates standardization, and creates an easier
troubleshooting scenario.

# Cookbooks and Recipes
## File Distribution

In the files sub-directory, you can place files that need to be pushed to
the nodes loading the cookbook.  This is especially useful when seeding
configuration data for packages.

This method is not to be used when you need to push a large file to a server.
In that case, you should use another method to retrieve the file from another
source.

For example, if you need to download a tar.gz file to compile an application
from, you should utilize an internal or external repository, such as a web or
ftp link.

# Cookbooks and Recipes
## Libraries

Libraries allow you to include arbitrary Ruby code, either to extend Chef's
language or to implement your own classes directly.

They are the secret sauce that will allow you to plug in to your existing
infrastructure and utilize it to inform how your systems are configured.

# Cookbooks and Recipes
## Recipes

Recipes are the fundamental configuration in Chef. Recipes encapsulate
collections of resources which are executed in the order defined to
configure the system

Recipes are an internal Ruby domain-specific language (DSL), but you do not
need to have experience with Ruby to write recipes.

Most things in Chef recipes will be resources to configure. Some things
in recipes will be Ruby syntax and helper code.

# Cookbooks and Recipes
## Lightweight Resources and Providers (LWRP)

In Chef, Resources represent a piece of system state and Providers are
the underlying implementation which brings them into that state. For
example, all database vendors support the abstract concept of database
creation, but the underlying implementation is different for each.

While typical Resources and Providers are implemented in Chef's core
using Ruby classes, implementing Lightweight Resources and Providers
(LWRP) is quick and easy, requiring less Ruby knowledge than their
heavier counterparts. (LWRP's also become Ruby classes, but this is
done for you, behind the scenes).

# Cookbooks and Recipes
## Templates

Templates get used all the time in Chef! We love templates. A template
is a file written in a markup language that allows one to dynamically generate
the file's final content based on variables or more complex logic.
Templates are commonly used to manage configuration files with Chef.
You utilize a template by adding a template resource to a recipe and
creating a corresponding ERB template in the cookbook.

The template can pull variables from the node attributes, or you can push
variables from the recipe based off of just about anything.

# Cookbooks and Recipes
## Metadata

Metadata about the cookbook is stored in metadata.rb.  The metadata
includes the cookbook name, maintaner, license, version, and any
dependancies this cookbook may have on other cookbooks.

*HEADS UP*  It is very important to bump your cookbook version
before modifying the cookbook.  If you do not bump this version,
chef environments that have the current version pinned will get the
change you make next time chef-client is run on the nodes that include
the cookbook you are editing.

We will dive more into versioning and why it's important in a few
minutes.

# Cookbooks and Recipes
## Metadata Sample

    @@@ruby
    maintainer       "Kameron Kenny"
    maintainer_email "kkenny@exacttarget.com"
    license          "Apache 2.0"
    description      "Installs/Configures buzz"
    long_description IO.read(File.join(File.dirname(__FILE__), 'README.md'))
    version          "0.0.2"
    #
    %w{java jpackage logrotate}.each do |p|
      depends p
    end


# Cookbooks and Recipes
## Metadata Sample (Cont.)

This particular cookbook depends on _java jpackage_ and _logrotate_.


Since we've declaired this, when chef-client runs and examines the
run list for the node, it will cache those defined cookbooks when
it goes through the process of collecting the cookbooks it needs to
complete the chef run.
