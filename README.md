# Hello Pulumi

This repository uses the [Get started with Pulumi & Google Cloud](https://www.pulumi.com/docs/iac/get-started/gcp/) tutorial to investigate how to deploy infrastructure to GCP in an automated fashion with Pulumi Cloud from a GitHub repository.
## Setup

This section describes the setup across all platforms and tools.

### Pulumi Cloud

I have a personal Pulumi Cloud account not an organization. In all the sections below my personal account was used instead of an orgaccount.

#### Configure OpenID Connect Between Pulumi Cloud and GitHub

Followed the Pulumi docs on [Configuring OpenID Connect for Github](https://www.pulumi.com/docs/pulumi-cloud/access-management/oidc/client/github/) to configure OIDC betwwen Pulumi Cloud and GitHub.

### GitHub

Installed the Pulumi GitHub App following the (`Pulumi docs on Installation and Configuration`](https://www.pulumi.com/docs/iac/packages-and-automation/continuous-delivery/github-app/#installation-and-configuration).

This is to enable Pulumi Cloud Deployments to be able to listen to events on my repos to be able to deploy IaC with Pulumi Cloud.
