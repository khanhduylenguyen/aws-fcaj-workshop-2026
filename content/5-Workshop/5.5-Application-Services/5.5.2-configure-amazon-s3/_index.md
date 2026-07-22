---
title : "Configure Amazon S3"
date : 2026-01-01
weight : 2
chapter : false
pre : " <b> 5.5.2. </b> "
---

## Configure Amazon S3

In this section, you will configure an Amazon S3 bucket to store product images for the Second-Hand Marketplace application.

Instead of storing image files on the application server, Amazon S3 provides scalable and durable object storage that can be accessed by the application running on Amazon ECS.

---

## Create an S3 Bucket

Navigate to:

**AWS Console → Amazon S3 → Buckets → Create bucket**

Configure the bucket using the following settings.

| Property | Value |
|----------|-------|
| Bucket name | *your-bucket-name* |
| AWS Region | ap-southeast-1 |
| Object Ownership | ACLs disabled |
| Block Public Access | Enabled |

After reviewing the configuration, choose **Create bucket**.

![Create Bucket](/images/5-Workshop/5.5-Application-Services/create-bucket.png)

---

## Upload Product Images

Open the bucket and choose **Upload**.

Upload one or more product images that will be used by the application.

After the upload is complete, verify that the objects appear in the bucket.

![Upload Objects](/images/5-Workshop/5.5-Application-Services/upload-images.png)

---

## Verify Bucket Content

Navigate to:

**Amazon S3 → Buckets → Your Bucket**

Confirm that the uploaded images are available in the bucket.

These images will be accessed by the application when displaying product information.

![Bucket Objects](/images/5-Workshop/5.5-Application-Services/bucket-object.png)

---

## Expected Result

After completing this section, you will have:

- An Amazon S3 bucket created.
- Product images uploaded successfully.
- Image objects stored in Amazon S3 and ready to be accessed by the application.