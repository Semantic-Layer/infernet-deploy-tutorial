# Ritual Infernet Deployment Tutorial

This repository provides a tutorial on deploying a Ritual Infernet node to Google Cloud. We will deploy the "hello-world" container located under `projects/hello-world/container`.

This example focuses on deploying a local container. Modifications have been made to [procure/prepare_files.sh](procure/prepare_files.sh) and [procure/gcp/scripts/node.tpl](procure/gcp/scripts/node.tpl) to work with local Docker container files.

If you have published your Infernet Docker container image, follow the original [instructions](https://github.com/ritual-net/infernet-deploy).

## Prerequisites
- [Install Terraform](https://developer.hashicorp.com/terraform/install?product_intent=terraform)
- Before you begin, ensure you have enabled the Compute Engine API on your Google Cloud project.

## Steps

### 1. Create a Google Cloud Service Account

Navigate to the GCP directory and create a service account using the script provided:

```bash
cd procure/gcp
chmod 700 create_service_account.sh
./create_service_account.sh
```

### 2. Copy the Example Configuration File

Make a copy of the example configuration file `terraform.tfvars.example`:

```bash
cd procure/gcp
cp terraform.tfvars.example terraform.tfvars
```

### 3. Modify `terraform.tfvars`

Update the following settings in the `terraform.tfvars` file:

- **service_account_email**: Replace the `project` in `ritual-deployer@project.iam.gserviceaccount.com` with your Google Cloud project ID. You can check the service account email from your Google Cloud project.

- **project**: Change this to your Google Cloud project ID.

- **ip_allow_ssh**: Add `35.235.240.0/20` (Google Cloud IAP) to allow access to the VM instance SSH in the browser.

- **region and zone**: Check your available zones and regions in Google Cloud "Compute Engines". The ones listed in the example might not be available for your project.

- **router.image**: The starter scripts in [procure/gcp/scripts/node.tpl](procure/gcp/scripts/node.tpl) use the Ubuntu Focal stable version. Ensure your image supports Ubuntu Focal stable installation. Choose an image that can install Ubuntu Focal stable, such as `ubuntu-minimal-2004-focal-v20240519`.

- **nodes**: List your node settings here. The node configuration name should match the name. For example, if you define a node named "my-node-123", the node configuration name should be `my-node-123.json`.

### 4. Deploy

Follow the remaining instructions to complete the deployment process.

```bash
# Initialize
cd procure
make init provider=gcp

# Print deployment plan
make plan provider=gcp

# Deploy
make apply provider=gcp

```

For any issues or further guidance, refer to the original [instructions](https://github.com/ritual-net/infernet-deploy).
