# Terraform Docker Image

[![Build and push Docker image](https://github.com/pbicskei/docker-terraform/actions/workflows/main.yml/badge.svg)](https://github.com/pbicskei/docker-terraform/actions/workflows/main.yml)

A Docker image for Terraform that allows you to run Terraform commands within a container.

## Introduction

Hi there! I am a DevOps engineer who loves to work with Terraform. I have found that working with Terraform in a containerized environment makes it easier to manage dependencies, isolate the environment, and avoid version conflicts. That's why I have created this Terraform Docker Image. 

## Usage

To locally build this Docker image, you can use either `docker build` or `docker buildx`.

### Using docker build

```bash
docker build --build-arg TARGETPLATFORM=linux/amd64 -t terraform .
```

### Using docker buildx

```bash
docker buildx build --platform linux/amd64 --build-arg TARGETPLATFORM=linux/amd64 -t terraform .
```

Once the image is built, you can run Terraform commands within the container by using the following command:

```bash
docker run -v $PWD:/data terraform <terraform command>
```

For example, to run `terraform init`, use the following command:

```bash
docker run -v $PWD:/data terraform init
```

### Using the pre-built image

If you don't want to build the image yourself, you can use the pre-built image from the Docker registry. The image is available at the following location:

```bash
docker pull docker.io/pbicskei/terraform
```

To use the pre-built image, simply run the following command:

```bash
docker run -v $PWD:/data pbicskei/terraform <terraform command>
```

For example, to run terraform init, use the following command:

```bash
docker run -v $PWD:/data pbicskei/terraform init
```

### Environment Variables

The following environment variables can be used to configure Terraform:

- `TF_LOG`: Specifies the log level for Terraform. The default value is "info".
- `TF_DATA_DIR`: Specifies the path to the Terraform data directory. The default value is ".terraform".
- `TF_PLUGIN_CACHE_DIR`: Specifies the path to the Terraform plugin cache directory. The default value is ".terraform.d/plugin-cache".
- `TF_CONFIG`: Specifies the path to the Terraform configuration file. The default value is "".
- `TF_VAR_*`: Specifies Terraform variables in the format of `TF_VAR_<variable_name>=<value>`.
- `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`: Required for Terraform to authenticate with AWS.
- `ARM_CLIENT_ID`, `ARM_CLIENT_SECRET`, `ARM_SUBSCRIPTION_ID`, `ARM_TENANT_ID`: Required for Terraform to authenticate with Azure.
- `GOOGLE_APPLICATION_CREDENTIALS`: Required for Terraform to authenticate with Google Cloud.

To set an environment variable when running Terraform in the container, use the `-e` option with the `docker run` command.

For example, to set the log level to "debug", use the following command:

```bash
docker run -e TF_LOG=debug -v $PWD:/data pbicskei/terraform <terraform command>
```

### Setting up Secrets in GitHub

In order to build and push your own container, you'll need to set up a couple of secrets in your GitHub repository.

Here's how to do that:

1. Go to the main page of your forked repository in GitHub.
2. Click on the "Settings" tab.
3. In the left-hand menu, click on "Secrets".
4. Click on the "New repository secret" button.
5. Enter `DOCKERHUB_USERNAME` as the name of the secret, and your Docker Hub username as the value.
6. Click the "Add repository secret" button to save.
7. Create another secret, this time with the name `DOCKERHUB_PASSWORD` and your Docker Hub password as the value.
8. Click the "Add repository secret" button to save.

With these secrets set up, your GitHub Actions workflow will have the necessary credentials to build and push the container to Docker Hub. If you don't set these secrets, the automated build will not work.
