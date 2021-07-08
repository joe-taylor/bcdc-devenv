> ⚠️ **Work in progress.** Please do not rely on this repo to create a functioning development environment. Although it's under active development, it's not yet in a useful state.

  
# BC Data Catalogue Dev Environment

## Contents

1. [Overview](#overview)
2. [Usage](#usage)
3. [Objective](#objective)
4. [Requirements](#requirements)
5. [What's provisioned](#whats-provisioned)
6. [Repos used](#repos-used)
7. [Enhancement ideas](#enhancement-ideas)

## Overview

We want to make the creation of BCDC development environments as easy as
typing a single command or two  `vagrant up` or perhaps `docker-compose up && vagrant up`.

We lean on Vagrant to create our development environment, with our Vagrantfile in turn leaning heavily on Ansible for provisioning. Provisioning is a work in
progress, far from complete; running `vagrant up` will not yet produce a
working instance of the BC Data Catalogue.

## Usage

`vagrant up` from the root directory, and any time you wish to run the associated ansible playbooks, `vagrant provision`.

## Objective

The Vagrantfile produces several CentOS 8 VirtualBox instances. The main two, titan and
enceladus, are for apps and databases, respectively. Here we use the
word "databases" loosely; we are not only referring to relational databases
(to wit, postgres), but also to NoSQL search platforms like Solr.

Additionally, the Vagrantfile creates a Jenkins box, vesta, for running various jobs
against these dev instances.

Finally, vagrant creates a box called europa to house the future UI.

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

**OS:** CentOS 8

**Purpose:** Hosting apps (old UI)

**Apps and services**

 - [CKAN](https://github.com/ckan/ckan/tree/ckan-2.7.5) (2.7.5)
   - [ckanext-bcgov](https://github.com/bcgov/ckanext-bcgov/tree/1.7.27) (1.7.27) *- use this repo's requirements.txt to populate the CKAN virtualenv*
 - [Datapusher](https://github.com/ckan/datapusher/tree/0.0.15) (0.0.15)
 - Redis (2.8.19)
 - Python (2.7.16 & 2.7.17)
    - virtualenv
    - pip (20.3.4)
 - uwsgi (2.0.17.1)
 - gunicorn (???)

**Python virtualenvs**

 - ckan (using python 2.7.17)
 - datapusher (using python 2.7.16)

**CKAN plugins**

- resource_proxy
- text_view
- recline_map_view
- recline_view
- datastore
- datapusher
- geo_view
- recline_graph_view
- geojson_view
- openapi_view
- pdf_view
- edc_rss
- edc_dataset
- edc_app
- edc_geo
- edc_ngeo
- edc_webservice
- edc_idir
- hierarchy_display
- hierarchy_form
- googleanalytics
- ga-report

To compare to the list of plugins installed in production, use the [status_show API call](https://catalogue.data.gov.bc.ca/api/3/action/status_show).

Source files are available on host via a Vagrant Synced Folder (NFS).

---

### enceladus.local (192.168.15.102)

**OS:** CentOS 8

**Purpose:** Hosting data (old and new UI)

**Apps and services**

 - Postgres (9.6)
 - Solr (7.2.1)

---

### vesta.local (192.168.15.150)

**OS:** CentOS 8

**Purpose:** Maintenance tasks and migration simulations

**Apps and services**

 - [ckanext-bcgov-schema](https://github.com/bcgov/ckanext-bcgov-schema/tree/master) (latest)
 - Jenkins (2.204.1)

Source files are available on host via a Vagrant Synced Folder (NFS).

---

### europa.local (192.168.15.201)

**OS:** CentOS 8

**Purpose:** Hosting apps (new UI)

**Apps and services**

 - CKAN
 - CKAN UI

Source files are available on host via a Vagrant Synced Folder (NFS).

## Repos used

 - ckan/ckan
 - bcgov/ckanext-bcgov
 - bcgov/ckanext-bcgov-schema
 - bcgov/ckanext-ga-report
 - bcgov/ckanext-googleanalytics
 - bcgov/ckanext-hierarchy
 - bcgov/ckanext-openapiviewer
 - bcgov/ckanext-sso
 - bcgov/ckanext-edc-rss
 - okfn/ckanext-envvars
 - bcdc/ckanext-edc-idir (gogs)

## Enhancement ideas

### Use docker-compose

This would complement - not replace - the existing VirtualBox VMs. It would be used for things like keycloak.

### Uses vagrant hostmanager plugin

This will complicate setup, but it may be worthwhile.

https://github.com/devopsgroup-io/vagrant-hostmanager
