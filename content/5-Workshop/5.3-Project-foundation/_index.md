---
title : "Prepare Project"
date : 2026-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

## Prepare Project

In this section, you will prepare the application source code before deploying it to AWS. This includes cloning the repository, installing the required dependencies, configuring the environment variables, and verifying that the application runs successfully in the local environment.

---

## Clone the Repository

Clone the project source code from GitHub.

```bash
git clone <repository-url>
cd <project-folder>
```

---

## Install Dependencies

Install all required Node.js packages.

```bash
npm install
```

Wait until the installation completes successfully.

---

## Configure Environment Variables

Create a `.env` file in the project root directory and configure the required environment variables.

Example:

```text
PORT=3000
MONGODB_URI=<your-mongodb-uri>
SESSION_SECRET=<your-session-secret>
AWS_REGION=ap-southeast-1
AWS_S3_BUCKET=<your-s3-bucket>
AWS_ACCESS_KEY_ID=<your-access-key>
AWS_SECRET_ACCESS_KEY=<your-secret-key>
```

![Configure Environment Variables](/images/5-Workshop/5.3-Project-foundation/env-file.png)

---

## Run the Application

Start the application.

```bash
npm start
```

If the application starts successfully, open your browser and navigate to:

```text
http://localhost:3000
```

![Run the Application](/images/5-Workshop/5.3-Project-foundation/run-localhost.png)

---

## Expected Result

After completing this section, you should have:

- Project source code cloned successfully.
- Required dependencies installed.
- Environment variables configured.
- Application running successfully in the local environment.