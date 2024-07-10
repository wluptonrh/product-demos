# OpenShift Demos
- These playbooks demonstrate the automation of basic VM lifecycle operations on OpenShift
- Why is it cool? By leveraging the AAP operator, we can quickly stand up our automation infrastructure and perform post-migration vm configurations from an alternative virtualization platform. Also, I'm lazy and this will automate (most) of the demo setup.
- NOTE: If this demo looks jank, that's because it is! This is very much a WIP, so pretty pretty please do not use this in production.

## Prerequisites
- You have an existing OpenShift Cluster with virtualization enabled or an Experience OpenShift Virtualization Roadshow environment on hand
- An existing Ansible Controller instance or (preferably) the Ansible Automation Platform Operator installed with a Controller instance running on your OCP Cluster
- You have created a Project named ```vmimported``` on your OpenShift cluster. Controller only references this project when populating its inventory... it's jank, I know.

## Setup Notes
  1. Follow the README in the root directory of this repo to pull collections with your Automation Hub credentials and create the default Setup JT. Worth noting that you will need to create a Controller Credential 
  2. Once you have created the Setup job template, add ```demo: openshift``` to the extra vars and launch. Once complete, the following resources will be created within AAP:

| Job Templates / Workflows | Description
|-----------|-------------|
| Application Deployment Workflow | This workflow combines the below job templates into a single execution. This is the recommended approach to leveraging the demo content |
| Provision VM | Will provision a RHEL 9 VM using a paramaterized VirtualMachine CR. All defaults for VM configurations are provided in the survey, though you can override these by disabling the survey and provide them directly in extra vars of the JT |
| Deploy Application | This will install one of two workloads: an apache webserver with custom web content or cockpit, to remotely manage your vm |
| Expose Application | Depending on the workload type you have chosen (apache or cockpit), this JT will create the necessary Service and Route definitions to make our workloads accessable outside the cluster network |

  3. Before we can execute the above templates, we need to populate the following Credentials that were created by the Setup template. 

| Credentials | Description
|-----------|-------------|
| OpenShift Default Credential | This Credential will allows Controller to perform tasks on the cluster. The easiest way to gather your the cluster ip and bearer token is by running ```oc cluster-info``` and ```oc whoami --show-token``` commands repspectively. |
| OpenShift Inventory Credential | This Credential serves the same purpose as above but is unique to the OCP Inventory Source that exists in AAP, which uses a slightly different method to authenticate to the cluster. In the future I will fix this to use a better auth mechanism, but this is my quick and easy workaround for now. |
| RHSM Credential | This credential will contain your Red Hat login info which is required to register your VM once it is provisioned. In the future, I plan to avoid this approach and leverage activation keys instead via cloud init ... but again it is an easy workaround for now :) |
| VM Credential | This credential will allow Controller to connect directly to the VM instance using an SSH keypair that you generate via ```ssh-keygen```. In this case, you only need to add the private key to this credential. The public key will be injected at provisioning time via the ```ssh_authorized_key``` extra var defined in the ```Provision VM``` Job Template.

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
