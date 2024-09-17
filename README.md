# Hello Pulumi

This repository uses the [Get started with Pulumi & Google Cloud](https://www.pulumi.com/docs/iac/get-started/gcp/) tutorial to investigate how to deploy infrastructure to GCP in an automated fashion with Pulumi Cloud from a GitHub repository.
## Setup

This section describes the setup across all platforms and tools.

### Pulumi Cloud

I have a personal Pulumi Cloud account not an organization. In all the sections below my personal account was used instead of an orgaccount.

From following the Get started with Pulumi & Google Cloud tutorial I had already created a project in Pulumi Cloud called `hello-pulumi` with a stack called `dev`.

#### Configure OpenID Connect Between Pulumi Cloud and GitHub

Followed the Pulumi docs on [Configuring OpenID Connect for Github](https://www.pulumi.com/docs/pulumi-cloud/access-management/oidc/client/github/) to configure OIDC betwwen Pulumi Cloud and GitHub.

#### Configure Deployment for `dev` Stack

After completing the [Configure Workload Identity Federation](#configure-workload-identity-federation) section below I configured the Pulumi Deployment settings for the `dev` stack in the `hello-pulumi` project following this [Pulumi docs](https://www.pulumi.com/docs/pulumi-cloud/deployments/get-started/#configure-deployment-settings).

### GitHub

Installed the Pulumi GitHub App following the (`Pulumi docs on Installation and Configuration`](https://www.pulumi.com/docs/iac/packages-and-automation/continuous-delivery/github-app/#installation-and-configuration).

This is to enable Pulumi Cloud Deployments to be able to listen to events on my repos to be able to deploy IaC with Pulumi Cloud.

### GCP

I had previously created a project in GCP with the Pulumi CLI, called `hello-pulumi`. The setup below is on this GCP project.

#### Configure Workload Identity Federation

I enabled the required APIS for workload identity federation on the GCP project following the second step of this [GCP docs](https://cloud.google.com/iam/docs/workload-identity-federation-with-other-providers#configure). As pointed out by the [Configuring OpenID Connect for Google Cloud](https://www.pulumi.com/docs/pulumi-cloud/access-management/oidc/provider/gcp/) Pulumi docs, those APIs are:
- Identity and Access Management (IAM) API, 
- Cloud Resource Manager API, 
- IAM Service account Credentials API and 
- Security Token Service API enabled.

Then I proceeded to configure workload identity federation following the Pulumi docs linked above. 

Then I moved to configure the Deployment settings for the `dev` stack of the Pulumi Cloud project.
