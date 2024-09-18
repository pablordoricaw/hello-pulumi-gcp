# Hello Pulumi

This repository uses the Python [Get started with Pulumi & Google Cloud](https://www.pulumi.com/docs/iac/get-started/gcp/) tutorial to investigate how to deploy infrastructure to GCP in an automated fashion with Pulumi Cloud from a GitHub repository.

## Methods

There are a couple methods to accomplish deploying Pulumi based infrastructure to GCP from a GitHub repository:

1. Pulumi Cloud backend + Pulumi Deployments + GitHub Repo
2. Pulumi Cloud backend + GitHub Actions + GitHub Repo
3. Self-managed backend in GCP + Pulumi Deployments + GitHub Repo
4. Self-managed backend in GCP + GitHub Actions + GitHub Repo

*This repository shows the setup for Pulumi Cloud backend + GitHub Actions + GitHub Repo*.

> [!NOTE]
> The first method, Pulumi Cloud backend + Pulumi Deployments + GitHub Repo, was attempted. However, I couldn't get the authentication for Pulumi Deployments and GCp setup properly.

## Setup

The setup was done as follows:

- Pulumi Cloud manages the IaC state
- GitHub Actions automates the deployment of changes to the IaC
- GCP is where the infra is deployed.

The subsections describe the setup to accomplish deploying Pulumi based infrastructure to GCP with GitHub Actions.

### Pre-requisites

To perform the setups I installed on my local machines:

- the Pulumi CLI following the [Pulumi Download & install Pulumi docs](https://www.pulumi.com/docs/iac/download-install/)
- the Google Cloud CLI (`gcloud` CLI) following the [GCP Install the gcloud CLI docs](https://cloud.google.com/sdk/docs/install)

### Pulumi Cloud

I have a personal Pulumi Cloud account not an organization.

> [!NOTE]
> In all the sections below my personal account was used whenever Pulumi asked for an organization account.

From following the Get started with Pulumi & Google Cloud tutorial I had already created a project in Pulumi Cloud called `hello-pulumi` with a stack called `dev`.

#### Pulumi Cloud 🤝 GitHub Actions

GitHub needs to authenticate with Pulumi Cloud that has the state. The authentication was setup through a Pulumi Cloud access token.

> [!WARNING]
> I couldn't get the authentication to work with OIDC for my personal account. It seems that the Pulumi auth GitHub Action expects a Pulumi org account instead of a personal.

The *Creating Personal Access Tokens* section of the following [Pulumi docs](https://www.pulumi.com/docs/pulumi-cloud/access-management/access-tokens/) walks through the steps on how to do create the token.

### GCP

I had previously created a project in GCP with the Pulumi CLI, called `hello-pulumi`. The setup below is on this GCP project.

#### Configure Workload Identity Federation

I configured direct workload identity federation following this section of the GCP auth GitHub Action [README](https://github.com/google-github-actions/auth?tab=readme-ov-file#preferred-direct-workload-identity-federation).

I enabled the required APIS for workload identity federation on the GCP project following the second step of this [GCP docs](https://cloud.google.com/iam/docs/workload-identity-federation-with-other-providers#configure). As pointed out by the [Configuring OpenID Connect for Google Cloud](https://www.pulumi.com/docs/pulumi-cloud/access-management/oidc/provider/gcp/) Pulumi docs, those APIs are:
- Identity and Access Management (IAM) API, 
- Cloud Resource Manager API, 
- IAM Service account Credentials API and 
- Security Token Service API enabled.

#### GCP 🤝 GitHub Actions

GitHub Actions needs to authenticate with GCP in order to deploy the IaC. The authentication was setup via the Workload Idnetity Federation setup in the previous section.

I followed the [(Preferred) Direct Workload Identity Federation](https://github.com/google-github-actions/auth?tab=readme-ov-file#direct-wif) section of the `google-github-actions/auth` GitHub action.
