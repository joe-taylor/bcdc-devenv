# BC Data Catalogue Dev Environment

We want to make the creation of BCDC development environments as easy as
typing a single command:  `vagrant up`.

The Vagrantfile leans heavily on ansible for provisioning. This is a work in
progress, far from complete; running `vagrant up` will not yet produce a
working instance of the BC Data Catalogue, but our goal is to eventually get
there.

## Objective

This should produce several CentOS 7 VirtualBox instances. The main two, titan and
enceladus, are for apps and databases, respectively. Here we use the
word "databases" loosely; we are not only referring to relational databases
(to wit, postgres), but also to NoSQL search platforms like Solr.

Additionally, we create a Jenkins box, vesta, for running various jobs
against these dev instances.

Finally, there's a box called europa which houses the future UI.

## Requirements

You'll need to have VirtualBox and Vagrant installed on your system.

For consistencies sake, the private network does not use DHCP, so the
following local IP addresses are hardcoded and must be available for
Virtualbox to bind to:

    192.168.15.101 titan.local titan
    192.168.15.102 enceladus.local enceladus
    192.168.15.150 vesta.local vesta
    192.168.15.201 europa.local europa

If you want to access servers by name from your host operating system, add the
corresponding entries to /etc/hosts. Alternatively, configure DNS.


## What's provisioned

### titan.local (192.168.15.101)

**Purpose:** Hosting apps (old UI)

**Apps and services**

 - [CKAN](https://github.com/ckan/ckan/tree/ckan-2.7.5) (2.7.5)
   - [ckanext-bcgov](https://github.com/bcgov/ckanext-bcgov/tree/1.7.27) (1.7.27)
 - [Datapusher](https://github.com/ckan/datapusher/tree/0.0.15) (0.0.15)
 - Redis (2.8.19)
 - Python (2.7.16)

---

### enceladus.local (192.168.15.102)

**Purpose:** Hosting data (old and new UI)

**Apps and services**

 - Postgres (9.6)
 - Solr (7.2.1)

---

### vesta.local (192.168.15.150)

**Purpose:** Maintenance tasks and migrations

**Apps and services**

 - [ckanext-bcgov-schema](https://github.com/bcgov/ckanext-bcgov-schema/tree/master) (latest)
 - Jenkins (7.2.1)

---

### europa.local (192.168.15.201)

**Purpose:** Hosting apps (new UI)

**Apps and services**

 - CKAN
 - CKAN UI
