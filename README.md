# BC Data Catalogue Dev Environment

We want to make spinning up working BCDC development environment as easy as
typing a single command:  `vagrant up`.

The Vagrantfile leans heavily on ansible for provisioning. The provisioning
part is a work in progress. Running `vagrant up` will not yet produce a
working instance of the BC Data Catalogue, but our goal is to eventually get
there.

## Objective

This should produce two CentOS 7 instances. One is for the app, and the other
is for the databases, with the term used loosely to not only encompass
postgres, but also NoSQL options like SOLR.

### titan.local (192.168.15.101)

**Purpose:** Apps

**Apps and services**

 - [CKAN](https://github.com/ckan/ckan/tree/ckan-2.7.5) (2.7.5)
   - [ckanext-bcgov](https://github.com/bcgov/ckanext-bcgov/tree/1.7.27) (1.7.27)
 - [Datapusher](https://github.com/ckan/datapusher/tree/0.0.15) (0.0.15)
 - Redis (latest)


### enceladus.local (192.168.15.102)

**Purpose:** Data

**Apps and services**

 - Postgres (9.6)
 - Solr (6.6.0)