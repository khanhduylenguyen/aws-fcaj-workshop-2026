---
title : "Configure AWS CodeBuild"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.9.1. </b> "
---

## Configure AWS CodeBuild

In this section, you will configure AWS CodeBuild to automate the build process of the Second-Hand Marketplace application.

AWS CodeBuild retrieves the application source code from GitHub, executes the build commands defined in the `buildspec.yml` file, builds the Docker image, and pushes the image to Amazon Elastic Container Registry (Amazon ECR).

---

## Create a CodeBuild Project

Navigate to:

**AWS Console → AWS CodeBuild → Build projects → Create build project**

Configure the project using the following settings.

| Property | Value |
|----------|-------|
| Project name | wed-mbdc-build |
| Source provider | GitHub |
| Repository | Your GitHub repository |

Choose **Next** to continue.

![Create Project](/images/5-Workshop/5.9-CI-CD/create-project.png)

---

## Configure the Build Environment

Configure the build environment.

| Property | Value |
|----------|-------|
| Environment image | Managed image |
| Operating system | Ubuntu |
| Runtime | Standard |
| Privileged mode | Enabled |

Enable **Privileged mode** to allow Docker image creation during the build process.

![Build Environment](/images/5-Workshop/5.9-CI-CD/build-environment.png)

---

## Configure the Build Specification

The project uses a `buildspec.yml` file stored in the source repository.

The build specification defines the commands required to:

- Authenticate with Amazon ECR.
- Build the Docker image.
- Tag the Docker image.
- Push the image to Amazon ECR.

![Buildspec](/images/5-Workshop/5.9-CI-CD/buildspec.png)

---

## Start a Build

Open the created CodeBuild project and choose **Start build**.

Wait until the build process finishes successfully.

![Build History](/images/5-Workshop/5.9-CI-CD/build-history.png)

---

## Verify the Build Result

Navigate to:

**AWS Console → CodeBuild → Build history**

Verify that:

- Build status is **Succeeded**.
- All build phases completed successfully.
- The Docker image has been pushed to Amazon ECR.

![Build Success](/images/5-Workshop/5.9-CI-CD/build-success.png)

---

## Expected Result

After completing this section, you will have:

- An AWS CodeBuild project connected to GitHub.
- A successful Docker image build.
- The Docker image pushed to Amazon ECR.
- An automated build workflow for the application.