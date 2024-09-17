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

### Pulumi Deployments & GCP Error

I ran into the error below when deploying the `hello-pulumi` project with Pulumi Cloud and GCP.

```
Type                   Name              Status                  Info
     pulumi:pulumi:Stack    hello-pulumi-dev  **failed**              1 error; 1 message
 +   ├─ gcp:storage:Bucket  my-bucket         **creating failed**     1 error
     └─ gcp:storage:Bucket  my-bucket
         **failed**              1 error
 
Diagnostics:
  pulumi:pulumi:Stack (hello-pulumi-dev):
    error: update failed
    Error creating bucket my-bucket-0274da0: Post "https://storage.googleapis.com/storage/v1/b?alt=json&prettyPrint=false&project=hello-pulumi-435400": oauth2/google: unable to generate access token: Post "https://iamcredentials.googleapis.com/v1/projects/-/serviceAccounts/spn-pulumi-cloud@hello-pulumi-435400.iam.gserviceaccount.com:generateAccessToken": oauth2/google: status code 400: {"error":"invalid_target","error_description":"The target service indicated by the \"audience\" parameters is invalid. This might either be because the pool or provider is disabled or deleted or because it doesn't exist."}
 
  gcp:storage:Bucket (my-bucket):
    error: 1 error occurred:
    	* Post "https://storage.googleapis.com/storage/v1/b?alt=json&prettyPrint=false&project=hello-pulumi-435400": oauth2/google: unable to generate access token: Post "https://iamcredentials.googleapis.com/v1/projects/-/serviceAccounts/spn-pulumi-cloud@hello-pulumi-435400.iam.gserviceaccount.com:generateAccessToken": oauth2/google: status code 400: {"error":"invalid_target","error_description":"The target service indicated by the \"audience\" parameters is invalid. This might either be because the pool or provider is disabled or deleted or because it doesn't exist."}
 
  gcp:storage:Bucket (my-bucket
):
    error:   sdk-v2/provider2.go:385: sdk.helper_schema: Post "https://storage.googleapis.com/storage/v1/b?alt=json&prettyPrint=false&project=hello-pulumi-435400": oauth2/google: unable to generate access token: Post "https://iamcredentials.googleapis.com/v1/projects/-/serviceAccounts/spn-pulumi-cloud@hello-pulumi-435400.iam.gserviceaccount.com:generateAccessToken": oauth2/google: status code 400: {"error":"invalid_target","error_description":"The target service indicated by the \"audience\" parameters is invalid. This might either be because the pool or provider is disabled or deleted or because it doesn't exist."}: provider=google-beta@7.38.0
 
Resources:
    1 unchanged
```

Additionally, I missed the deployment happening right next to my code. I wasn't a big fan of having to go into Pulumi Cloud to check the deployment.

The PR chatbot feature was cool though...

### GitHub Actions to the Rescue!

After a few unsuccessful tries to fix the issue with Pulumi Deployments and GCP cloud, I gave GitHub Actions a run.

#### GitHub and Pulumi Cloud

First thing, I stored a Pulumi Cloud access token in the secrets of the repo for GitHub actions to authenticate with Pulumi Cloud. This token expires on 12/20/204

#### GitHub and GCP

I created a new service account called `github` and a new workload identity pool called `github-actions-pool` following the same instructions that I did to create the Pulumi ones.

Then in the `pulumi.yaml` GitHub Actions workflow I used these to authenticate with GCP. The following 

- [GitHub docs](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/configuring-openid-connect-in-google-cloud-platform) 
- [GCP docs](https://cloud.google.com/iam/docs/workload-identity-federation-with-deployment-pipelines#by-pool)

were useful for this step.
