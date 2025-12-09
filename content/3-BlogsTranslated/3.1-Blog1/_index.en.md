---
title: "Blog 1 - English Reverse Translation"
date: "2025-12-07"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# How AWS and Intel make Large Language Models more accessible and cost-effective with DeepSeek

by Dylan Souvage, Vishwa Gopinath Kurakundi, and Anish Kumar on 07 APRIL 2025 in [Amazon EC2](https://aws.amazon.com/blogs/apn/category/compute/amazon-ec2/), [Artificial Intelligence](https://aws.amazon.com/blogs/apn/category/artificial-intelligence/), [Compute](https://aws.amazon.com/blogs/apn/category/compute/), [Generative AI](https://aws.amazon.com/blogs/apn/category/artificial-intelligence/generative-ai/), [Generative AI](https://aws.amazon.com/blogs/apn/category/generative-ai-2/), [Partner solutions](https://aws.amazon.com/blogs/apn/category/post-types/partner-solutions/), [Responsible AI](https://aws.amazon.com/blogs/apn/category/responsible-ai/) [Permalink](https://aws.amazon.com/blogs/apn/how-aws-and-intel-make-llms-more-accessible-and-cost-effective-with-deepseek/)

*By Anish Kumar, AI Software Engineering Manager – Intel*
*Dylan Souvage, Solutions Architect – AWS*
*Vishwa Gopinath Kurakundi, Solutions Architect – AWS*

Enterprises are seeking effective ways to deploy Large Language Models (LLMs). They want to leverage the power of LLMs while also needing solutions that balance performance and cost.

At the [recent AWS re:Invent conference in Las Vegas](https://aws.amazon.com/blogs/aws/top-announcements-of-aws-reinvent-2024/), Andy Jassy, CEO of Amazon, shared three valuable lessons from Amazon's internal experience building more than 1,000 GenAI applications:

* Cost-effectiveness at scale is key for GenAI applications.
* Building effective GenAI applications requires careful consideration.
* Model diversity is essential – there is no "one model fits all" solution.

These lessons guide how AWS collaborates with customers to deploy GenAI. At AWS, we recognize that flexibility and choice are important for customers. Andy Jassy also emphasized that AWS's diverse LLM portfolio helps customers easily find the right tool for their specific needs. Through deep collaboration with partners like Intel, AWS continuously expands its curated LLM portfolio, enhancing accessibility for customers.

## **Intel and AWS**

The [collaboration between AWS and Intel began in 2006](https://aws.amazon.com/blogs/aws/happy-15th-birthday-amazon-ec2/), when we launched [Amazon Elastic Compute Cloud (EC2)](https://aws.amazon.com/ec2/) using [Intel chips](https://aws.amazon.com/intel/). Over 19 years, this partnership has grown strong to provide cloud services that optimize costs, simplify operations, and meet changing business needs. [Intel® Xeon®](https://www.intel.com/content/www/us/en/products/details/processors/xeon.html) Scalable processors are the foundation for many cloud computing services on AWS. EC2 instances using Intel Xeon processors have the [broadest availability, global reach, and highest availability](https://aws.amazon.com/ec2/instance-types/) in AWS regions. In September 2024, AWS and Intel announced [a multi-year, multi-billion dollar joint investment agreement to design custom chips including products and wafers from Intel.](https://press.aboutamazon.com/aws/2024/9/intel-and-aws-expand-strategic-collaboration-helping-advance-u-s-based-chip-manufacturing) This extends the long-term collaboration between the two companies, helping customers run nearly any type of workload and accelerate the performance of artificial intelligence (AI) applications.

## **DeepSeek**

AWS and Intel are collaborating to help enterprises access and deploy cost-effective LLMs. A new trend is distilled language models, which maintain high performance while requiring fewer resources. These models can run directly on CPUs, also known as Small Language Models (SLMs). Training and inferring SLMs on CPUs helps deploy high-performance AI within reasonable time and cost constraints. DeepSeek models (developer of DeepSeek-R1) are rapidly becoming popular due to their efficiency, low cost, and open-source nature, allowing free deployment in applications. Additionally, DeepSeek provides distilled versions, smaller "student" models trained to replicate the response quality of larger "teacher" models but consuming fewer resources.

Amazon EC2 is a cost-effective platform for deploying Large Language Models (LLMs), while providing specialized instance types running on Intel® Xeon® Scalable processors, suitable for deploying optimized models like the DeepSeek-R1 distilled version. 4th generation and newer Intel® Xeon® CPUs are equipped with Advanced Matrix Extensions (AMX) accelerators, significantly improving LLM workload performance by accelerating matrix multiplication, a core component in LLM inference. These AMX accelerators deliver superior processing performance while integrating with open standards like oneAPI, helping enterprises deploy Generative Artificial Intelligence (Generative AI) applications with reasonable costs, high scalability, faster result acquisition time, and lower total cost of ownership (TCO).

Amazon EC2 also provides excellent flexibility and scalability by supporting various deployment configurations, including virtual LLM (vLLM) models that can seamlessly integrate with Docker based on the [Hugging Face platform](https://huggingface.co/docs/hub/en/index). In [this accompanying tutorial](https://community.aws/content/2kveh5D6VsyPO5MKYfsoZBOmD21/deploying-deepseek-r1-distill-llama-8b-on-aws-m7i-instance), we'll see in detail step-by-step how to quickly deploy the DeepSeek-R1-Distill-Llama-8B model on Amazon EC2 m7i.2xlarge instance, using Intel® Xeon® Scalable processor with 8 vCPU and 32 GB memory. The article provides detailed guidance on configuring Amazon EC2 to deploy the model, while building Docker container for vLLM on CPU – including Intel's CPU optimizations like Intel Extension for PyTorch. This extension ensures LLM inference processes are optimized to run efficiently on 4th generation and newer Intel® Xeon® processors, and the article concludes with testing inference after the model is deployed.

## **Conclusion**

Enterprises can deploy custom or open-source LLMs, including Distilled DeepSeek-R1, on AWS through managed services like Amazon Bedrock and Amazon SageMaker, or deploy directly on Amazon EC2, depending on specific needs. The collaboration between AWS and Intel is driving the development of the Generative Artificial Intelligence field, combining Intel's advanced semiconductor technology with AWS's powerful cloud infrastructure to deliver accessible and cost-effective AI solutions.

To learn more about AWS's Generative AI field, visit [AWS's Machine Learning blog.](https://aws.amazon.com/blogs/machine-learning/)

![Connect with Intel][image1]

---

## **Intel – AWS Partner Spotlight**

Intel and Amazon Web Services (AWS) have collaborated for over 19 years to develop flexible and optimized software technologies, serving enterprise critical workloads. This collaboration enables AWS partners to support customers in migrating and modernizing their applications and infrastructure – helping reduce costs and complexity, accelerate business outcomes, while scaling to meet current and future computing needs.
[Contact Intel](https://partnercentral.awspartner.com/PartnerConnect?id=001E000000aSLc8IAG&source=Blog&campaign=) | [Partner Overview](https://aws.amazon.com/partners/find/partnerdetails/?id=001E000000aSLc8IAG) | [AWS Marketplace](https://aws.amazon.com/marketplace/featured-seller/intel)

TAGS: [Amazon EC2](https://aws.amazon.com/blogs/apn/tag/amazon-ec2/), [AWS Competency Partners](https://aws.amazon.com/blogs/apn/tag/aws-competency-program/), [AWS Partner Solution](https://aws.amazon.com/blogs/apn/tag/aws-partner-solution/), [Generative AI](https://aws.amazon.com/blogs/apn/tag/generative-ai/), [Intel](https://aws.amazon.com/blogs/apn/tag/intel/)