---
title : "Configure AWS Secrets Manager"
date : 2026-01-01
weight : 3
chapter : false
pre : " <b> 5.5.3. </b> "
---

## Configure AWS Secrets Manager

In this section, you will configure AWS Secrets Manager to securely store sensitive information used by the Second-Hand Marketplace application.

Instead of embedding secrets directly in the application source code, AWS Secrets Manager provides a secure and centralized way to manage sensitive configuration values.

---

## Create a Secret

Navigate to:

**AWS Console → AWS Secrets Manager → Secrets → Store a new secret**

Select **Other type of secret**.

Configure the secret using the following information.

| Property | Value |
|----------|-------|
| Secret type | Other type of secret |
| Key | MONGODB_URI |
| Value | MongoDB Atlas connection string |

Choose **Next** to continue.

![Create Secret](/images/5-Workshop/5.5-Application-Services/create-secret.png)

---

## Configure Secret Details

Provide a name for the secret.

Example:

| Property | Value |
|----------|-------|
| Secret name | production/mongodb |
| Description | MongoDB connection string |

Choose **Next** and keep the remaining settings as default.

![Secret Details](/images/5-Workshop/5.5-Application-Services/secret-details.png)

---

## Verify the Secret

Navigate to:

**AWS Console → Secrets Manager → Secrets**

Confirm that the newly created secret appears in the list.

The application will retrieve this secret during deployment on Amazon ECS.

![Secret List](/images/5-Workshop/5.5-Application-Services/secret-list.png)

---

## Expected Result

After completing this section, you will have:

- A secret created in AWS Secrets Manager.
- The MongoDB connection string securely stored.
- A secret ready to be used by the Amazon ECS task.