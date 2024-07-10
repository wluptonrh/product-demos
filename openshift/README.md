# OpenShift Demos
- These playbooks demonstrate the automation of basic VM lifecycle operations on OpenShift

## Prerequisites
- You have an existing OpenShift Cluster with virtualization enabled or an Experience OpenShift Virtualization Roadshow environment on hand
- An existing Ansible Controller instance or the Ansible Automation Platform Operator installed with a Controller instance running

## Setup Notes
  1. Follow general setup instructions in the root directory of this repo to pull collections with your Automation Hub credentials and create the default Setup JT
  2. Once you have created the Setup job template, add ```demo: openshift``` to the extra vars and launch. Once complete, the following resources will be created within AAP:
     



## Table of Contents
- [OpenShift Demos](#openshift-demos)
  - [Table of Contents](#table-of-contents)
  - [About These Demos](#about-these-demos)
    - [Jobs](#jobs)
  - [Pre Setup](#pre-setup)

## About These Demos
This category of demos shows examples of openshift operations and management with Ansible Automation Platform. The list of demos can be found below. See the [Suggested Usage](#suggested-usage) section of this document for recommendations on how to best use these demos.

### Jobs
- [**OpenShift / Dev Spaces**](devspaces.yml) - Install and deploy dev spaces on OCP cluster. After this job has run successfully, login to your OCP cluster, click the application icon (to the left of the bell icon in the top right) to access Dev Spaces

## Pre Setup
This demo requires an OpenShift cluster to deploy to. If you do not have a cluster to use, one can be requested from [demo.redhat.com](https://demo.redhat.com).
- Search for the [Red Hat OpenShift Container Platform 4.12 Workshop](https://demo.redhat.com/catalog?item=babylon-catalog-prod/sandboxes-gpte.ocp412-wksp.prod&utm_source=webapp&utm_medium=share-link) item in the catalog and request with the number of users you would like for Dev Spaces.
- Login using the admin credentials provided. Click the `admin` username at the top right and select `Copy login command`.
- Authenticate and click `Display Token`. This information will be used to populate the OpenShift Credential after you run the setup.
