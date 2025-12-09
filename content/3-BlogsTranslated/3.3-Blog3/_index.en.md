---
title: "Blog 3 - English Reverse Translation"
date: "2025-12-07"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Simultaneously serving multiple Large Language Models (LLMs) with LoRAX

[Omitted long context line]

[Omitted long context line]

[Omitted long context line]

[Omitted long context line]

[Omitted long context line]

![][image1]

## **Why choose LoRAX for LoRA deployment on AWS?**

The growing popularity of LLM fine-tuning has led to the emergence of various inference container methods for deploying LoRA adapters on AWS. The two most prominent methods used by our customers are LoRAX and [vLLM](https://github.com/vllm-project/vllm).

[Omitted long context line]

[Omitted long context line]

## **Solution Overview**

The LoRAX inference container can be deployed on a single EC2 G6 instance, and models along with adapters can be loaded from [Amazon Simple Storage Service](https://aws.amazon.com/s3/) (Amazon S3) or Hugging Face. The following diagram illustrates the architecture of this solution.

![][image3]

## **Prerequisites**

To follow this tutorial, you need access to the following requirements:

* An AWS account
* Appropriate permissions to deploy EC2 G6 instances. LoRAX is built with the purpose of using NVIDIA CUDA technology, and the EC2 G6 instance family is the most cost-effective type with the latest NVIDIA CUDA accelerators. Specifically, G6.xlarge is the most cost-effective choice for this tutorial at the time of writing. Please ensure that the quota has been increased before deployment.
[Omitted long context line]

## **Hands-on Tutorial**

This article will guide you through creating an EC2 instance, downloading and deploying the container image, while storing a pre-trained language model along with custom adapters from Amazon S3. Follow the prerequisite checklist to ensure you can deploy this solution correctly.

### **Detailed Server Configuration**

[Omitted long matching line]

[Omitted long matching line]

Depending on the parameters of the language model, you need to adjust the [Amazon Elastic Block Store](https://aws.amazon.com/ebs/) (Amazon EBS) storage capacity to have enough space for the base model and adapter weights.

To set up your inference server, follow these steps:

1. In the [Amazon EC2 management console](https://console.aws.amazon.com/ec2/), select **Launch instances**, as illustrated in the image below.![][image4]
2. For the name, enter LoRAX - Inference Server.
3. To open [AWS CloudShell](https://aws.amazon.com/cloudshell/), in the lower left corner of the [AWS Management Console](https://aws.amazon.com/console/) select CloudShell, as illustrated in the following screenshot.![][image5]

[Omitted long matching line]

[Omitted long matching line]

## **Cost Comparison and Scalability Advice**

[Omitted long context line]

In the previous example, the adapters obtained from the training process have a size of approximately 5 MB.

[Omitted long context line]

[Omitted long context line]

[Omitted long context line]

[Omitted long context line]

## **Storing LoRAX server for multiple models in production environment**

[Omitted long context line]

![][image17]

## **Cleanup**

In this tutorial, we have created security groups, an S3 bucket, a SageMaker notebook instance (optional), and an EC2 inference server. It's important to terminate the resources created during the process to avoid incurring additional costs:

1. Delete the S3 bucket
2. Terminate the EC2 inference server
3. Terminate the SageMaker notebook instance

## **Conclusion**

[Omitted long context line]

[Omitted long context line]

[Omitted long context line]

[Omitted long context line]

[Omitted long context line]