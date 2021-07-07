# BC Data Catalogue Dev Environment

We want to make spinning up working BCDC development environment as easy as
typing a single command:  `vagrant up`.

The Vagrantfile leans heavily on ansible for provisioning. The provisioning
part is a work in progress. Running `vagrant up` will not yet produce a
working instance of the BC Data Catalogue, but we're getting there.

## Objective

This should produce two CentOS 7 instances. One is for the app, and the other
is for the databases, with the term used loosely to not only encompass
postgres, but also NoSQL options like SOLR.

### titan.local (192.168.15.101)

|-------|-------------------|
|Purpose| Hosting CKAN Apps |
|-------|-------------------|

### enceladus.local (192.168.15.102)

|-------|--------------------------------|
|Purpose| Hosting Database and Datastore |
|-------|--------------------------------|
