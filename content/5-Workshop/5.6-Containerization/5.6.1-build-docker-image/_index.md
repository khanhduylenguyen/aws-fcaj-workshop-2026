---
title : "Build Docker Image"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.6.1. </b> "
---

## Build Docker Image

In this section, you will create a Dockerfile and build a Docker image for the Second-Hand Marketplace application.

Docker packages the application and its dependencies into a portable container, ensuring a consistent runtime environment across development and production.

---

## Create the Dockerfile

Open the project folder and create a file named **Dockerfile** in the root directory.

The Dockerfile used in this project is shown below.

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

Save the Dockerfile after completing the configuration.

![Dockerfile](/images/5-Workshop/5.6-Containerization/dockerfile.png)

---

## Build the Docker Image

After creating the Dockerfile, open a terminal in the project root directory and build the Docker image.

Run the following command.

```bash
docker build -t secondhand-marketplace .
```

Docker performs the following operations during the build process:

1. Downloads the required Node.js base image if it is not available locally.
2. Creates the application working directory inside the container.
3. Copies the project files into the container.
4. Installs all application dependencies using **npm install**.
5. Packages the application into a Docker image.

When the build completes successfully, Docker displays a message similar to the following.

```text
Successfully built <IMAGE_ID>
Successfully tagged secondhand-marketplace:latest
```

To verify that the image was created successfully, run:

```bash
docker images
```

The command displays all Docker images stored on the local machine. Confirm that the newly created image appears in the list with the **latest** tag.

---

## Expected Result

After completing this section, you will have:

- A Dockerfile created for the application.
- A Docker image built successfully.
- A local Docker image ready to be pushed to Amazon ECR.