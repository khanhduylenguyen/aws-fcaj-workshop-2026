---
title : "Configure MongoDB Atlas"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.5.1. </b> "
---

## Configure MongoDB Atlas

In this section, you will configure MongoDB Atlas to serve as the database for the Second-Hand Marketplace application.

MongoDB Atlas is a fully managed cloud database that stores application data such as users, products, categories, and orders.

---

## Create a Database Cluster

Sign in to MongoDB Atlas and navigate to:

**Deployment → Database**

Create a cluster or use an existing cluster for the project.

After the cluster is created, verify that its status is **Available**.

![MongoDB Cluster](/images/5-Workshop/5.5-Application-Services/mongodb-cluster.png)

---

## Create a Database User

Navigate to:

**Security → Database Access**

Create a database user with the required permissions.

Example configuration:

| Property | Value |
|----------|-------|
| Authentication Method | Password |
| Username | admin |
| Database User Privileges | Atlas Admin |

Save the username and password for later use.

![Database User](/images/5-Workshop/5.5-Application-Services/database-user.png)

---

## Configure Network Access

Navigate to:

**Security → Network Access**

Add the IP addresses that are allowed to connect to the database.

For development purposes, you may temporarily allow access from all IP addresses.

| Property | Value |
|----------|-------|
| Access List Entry | 0.0.0.0/0 |

After deployment, replace this with the appropriate public IP address or CIDR range.

![Network Access](/images/5-Workshop/5.5-Application-Services/network-access.png)

---

## Obtain the Connection String

Open the cluster and select **Connect**.

Choose **Drivers** and copy the MongoDB connection string.

Example:

```text
mongodb+srv://admin:<password>@cluster0.xxxxx.mongodb.net/secondhand
```

This connection string will be stored securely using AWS Secrets Manager in a later section.

![Connection String](/images/5-Workshop/5.5-Application-Services/connection-string.png)

---

## Expected Result

After completing this section, you will have:

- A MongoDB Atlas cluster ready for use.
- A database user configured.
- Network access configured.
- A MongoDB connection string ready for the application.