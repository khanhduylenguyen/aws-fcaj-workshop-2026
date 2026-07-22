---
title : "Configure Amazon CloudWatch"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.10.1. </b> "
---

# Configure Amazon CloudWatch

In this section, you will configure Amazon CloudWatch to monitor the deployed application running on Amazon ECS.

Amazon CloudWatch provides monitoring capabilities by collecting metrics from AWS resources and evaluating them against user-defined thresholds. This enables administrators to detect abnormal resource usage, observe application performance, and respond quickly when issues occur.

In this project, CloudWatch is used to monitor the CPU utilization of the ECS Service by creating a CloudWatch Alarm.

---

## Configure CloudWatch Logging

Amazon ECS supports integration with Amazon CloudWatch through the **awslogs** log driver. The Task Definition specifies the CloudWatch Log Group, AWS Region, and Stream Prefix that are used when the container sends runtime logs to CloudWatch.

The following configuration is defined inside the ECS Task Definition.

```json
"logConfiguration": {
  "logDriver": "awslogs",
  "options": {
    "awslogs-group": "/ecs/wed-mbdc-task",
    "awslogs-create-group": "true",
    "awslogs-region": "ap-southeast-1",
    "awslogs-stream-prefix": "ecs"
  }
}
```

![CloudWatch Logging Configuration](/images/5-Workshop/5.10-Monitoring/ecs-log-configuration.png)

---

## Create a CloudWatch Alarm

To continuously monitor the health of the application, a CloudWatch Alarm is configured for the Amazon ECS Service.

The alarm evaluates the average CPU utilization every five minutes. If the CPU usage exceeds the configured threshold, CloudWatch changes the alarm state from **OK** to **ALARM**, allowing administrators to identify abnormal resource consumption and investigate potential performance issues.

The CloudWatch Alarm is configured using the following settings.

| Property | Value |
|----------|-------|
| Alarm name | production-service-cpu-alarm |
| Namespace | AWS/ECS |
| Metric | CPUUtilization |
| Threshold | 80% |
| Evaluation period | 5 minutes |
| Cluster | production-cluster |
| Service | production-service |

![CloudWatch Alarm List](/images/5-Workshop/5.10-Monitoring/cpu-alarm-list.png)

---

## Verify the Alarm Status

After the alarm is created, Amazon CloudWatch continuously monitors the CPU utilization of the ECS Service.

The alarm remains in the **OK** state while CPU usage stays below the configured threshold. If the CPU utilization exceeds 80% during the evaluation period, CloudWatch automatically changes the alarm state to **ALARM**.

The alarm detail page also displays additional monitoring information, including:

- Current alarm state.
- CPU utilization graph.
- Threshold configuration.
- Evaluation period.
- Cluster name.
- ECS Service name.
- Namespace and monitored metric.

This information helps administrators understand the current operating condition of the deployed application and quickly identify potential performance bottlenecks.

![CloudWatch Alarm Details](/images/5-Workshop/5.10-Monitoring/cpu-alarm-details.png)

---

## Expected Result

After completing this section, you will have:

- Amazon ECS configured to use the **awslogs** log driver.
- A CloudWatch Alarm monitoring CPU utilization for the ECS Service.
- Real-time visibility into the health of the deployed application.
- A monitoring mechanism that helps detect abnormal CPU usage and supports application maintenance.