# Data Bag

Data bags are most simply put as _bags of data_.

These _bags of data_ are made up of json attributes.

For example, lets say we have a data bag called _zones_.
In that data bag, we have zone information for two different
domain names in two seperate json files: abc-com.json, xyz-com.json.

In those json files, you could have all the NS records for your domain,
then utilize a cookbook recipe to configure your name servers based
off of that information.

This architecture allows us to have consistent sets of data accross all
nodes configured with these attributes.

# Data Bag
## Loading data bags

Before loading the data bags, you normally will do a search.  You will
search for specific attributes matching a specified pattern, then load
that particular set of data from the data bag.
