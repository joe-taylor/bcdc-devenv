# BC Data Catalogue Dev Environment

We want to make spinning up working BCDC development environment as easy as
typing a single command:  `vagrant up`.

The Vagrantfile leans heavily on ansible for provisioning. This is a work in
progress, far from complete; running `vagrant up` will not yet produce a
working instance of the BC Data Catalogue, but our goal is to eventually get
there.

## Objective

This should produce several CentOS 7 instances. The main two, titan and
enceladus, are for apps and databases, respectively. Here we use the
word "databases" loosely; we are not only referring to relational databases
(specifically, postgres), but also NoSQL search platforms like Solr.

Additionally, we create a Jenkins box, vesta.local, for running various jobs
against these dev instances.

Finally, there's a box called europa.local which houses the future UI.

## Provisioned Boxes

### titan.local (192.168.15.101)

**Purpose:** Hosting apps (old UI)

**Apps and services**

 - [CKAN](https://github.com/ckan/ckan/tree/ckan-2.7.5) (2.7.5)
   - [ckanext-bcgov](https://github.com/bcgov/ckanext-bcgov/tree/1.7.27) (1.7.27)
 - [Datapusher](https://github.com/ckan/datapusher/tree/0.0.15) (0.0.15)
 - Redis (latest)

---

### enceladus.local (192.168.15.102)

**Purpose:** Hosting data (old and new UI)

**Apps and services**

 - Postgres (9.6)
 - Solr (6.6.0)

---

### vesta.local (192.168.15.150)

**Purpose:** Maintenance tasks and migrations

**Apps and services**

 - [ckanext-bcgov-schema](https://github.com/bcgov/ckanext-bcgov-schema/tree/master)
 - Jenkins

---

### europa.local (192.168.15.201)

**Purpose:** Hosting apps (new UI)

**Apps and services**

 - CKAN
 - CKAN UI