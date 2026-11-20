
# Prerequisites and AWS Services Overview

## Prerequisites

### `Docker`

`Docker`: Containerization is a key practice for creating reproducible and isolated production environments. `Docker` will be used to package the `vLLM` server and the Deepseek R1 model into a portable image that can be deployed consistently on any EC2 instance running `Docker`.

### `Git`

`Git`: You will need `Git` to clone repositories, such as the official `vLLM` repository or to manage your own infrastructure-as-code scripts.

Having these tools installed and properly configured on your local machine is fundamental for executing the deployment steps outlined later in this guide.

## Understanding AWS Services Involved (EC2, EKS, Lambda, etc.)

A foundational understanding of the core AWS services that form the backbone of the deployment architecture is critical for both implementation and long-term maintenance. While this guide focuses on a specific deployment pattern, knowing the role of each service helps in troubleshooting and adapting the architecture to future needs.

Here is a brief overview of the primary services you will interact with:

### Amazon EC2 (Elastic Compute Cloud)

Amazon EC2 (Elastic Compute Cloud) provides the virtual servers (instances) where the Deepseek R1 model will be run. You will need to select an appropriate instance type, typically one with a powerful GPU (like the g4dn, g5, or p4d series) to handle the computational demands of LLM inference efficiently.

### Amazon ECR (Elastic Container Registry)

Amazon ECR (Elastic Container Registry) is a managed `Docker` container registry where you will store your custom `Docker` image containing `vLLM` and the model. Using ECR simplifies the process of deploying containerized applications to EC2 or other AWS container services.

### Amazon S3 (Simple Storage Service)

Amazon S3 (Simple Storage Service) is an object storage service that is ideal for storing large files. You can use it to store the Deepseek R1 model weights, which can then be downloaded to the EC2 instance at runtime. This decouples the model artifacts from the container image, making the image lighter and more reusable.

### Amazon VPC and Security Groups

Amazon VPC and Security Groups allow you to create an isolated network environment in the AWS cloud. Security Groups act as a virtual firewall for your EC2 instances, controlling inbound and outbound traffic. You will configure a security group to allow HTTPS traffic to the `vLLM` API endpoint while restricting other access.

### Amazon CloudWatch

Amazon CloudWatch is the primary monitoring service on AWS. It can be configured to collect logs from your `vLLM` server and track metrics like GPU utilization, memory usage, and request latency, which are essential for maintaining a healthy production service.

While services like Amazon EKS (Elastic Kubernetes Service) or AWS Lambda could also be used for deploying LLMs, this guide will focus on a more direct approach using EC2 to provide a clear, foundational understanding of the core components.

## Step-by-step Deployment Guide
