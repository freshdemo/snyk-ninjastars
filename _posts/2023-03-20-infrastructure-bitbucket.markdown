---
layout: post
title:  "Bitbucket Server"
date:   2021-11-19 16:34:10 -0400
categories: infrastructure 
tags: infrastructure
---

This guide walks through how to instantiate a Virtual Machine (VM) in Google Cloud Platform (GCP) with a running Bitbucket container, using Terraform.

Getting this type of infrastructure setup on a mac is often painful or not possible. Ideally this deploys enough that you're able to destroy it when done using it (or at least shut it down).


### Step 1 - Install Terraform

Terraform is a cross platform automation framework most of our customers will be using. Install the CLI by following the steps here, https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli.

    brew tap hashicorp/tap
    brew install hashicorp/tap/terraform
    terraform -install-autocomplete

### Step 2 - Install GCP Command Line

Follow this guide to install the GCP SDK, https://cloud.google.com/sdk/docs/install-sdk. The basic steps are to download the file from the afformentioned site and;

    tar -xf google-cloud-sdk-329.0.0-darwin-x86_64.tar.gz

Then to get setup run the command below and follow the prompts;

    gcloud init


### Step 3 - Deploy

This repository includes terraform and docker configurations to instantiate the VM in GCP, then run a Bitbucket container that has been secured with SSL onit.

    git clone https://github.com/stephen-snyk/snyk-box.git

See the README on the repo for the latest details on variables that need to be set.

Once the variables are set, run the following to initialize the project, create a plan file from the .tf configuration files, and finally apply that plan file to build the infrastructure;

    terraform init
    terraform plan --out "first.plan"
    terraform apply "first.plan

To destroy all of the infrastructure built in the previous step, use;

    terraform destroy

And to interate through configuration changes you only need the terraform plan, apply and destroy steps.

### Step 4 - Personalize Setup

At this point you should have Bitbucket server running with a public IP assigned. You can find the IP on your VM in GCP and it is also output in the terraform apply phase. Navigate to;

    https://<public IP>

From here you will go through the basic steps of licensing Bitbucket (you'll need to create an account if you don't have one), creating an administrative user, and adding repositories.

### Step 5 - Snyk Broker

Snyk Broker should also be deployed at this point. After adding a repository to Bitbucket, attempt to add a project from the Snyk UI.


<!-- Cloudflare Web Analytics --><script defer src='https://static.cloudflareinsights.com/beacon.min.js' data-cf-beacon='{"token": "3ff248322f9d497f8802ebf7d47130c1"}'></script><!-- End Cloudflare Web Analytics -->
