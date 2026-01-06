---
title: "A Beginner's Guide to Terraform"
seoTitle: "Terraform Basics: A Starter's Handbook"
seoDescription: "Learn the basics of Terraform and Infrastructure as Code with practical examples and core concepts for beginners"
datePublished: Tue Jan 06 2026 09:29:05 GMT+0000 (Coordinated Universal Time)
cuid: cmk2e0vu2002c02jm9bbq5s06
slug: a-beginners-guide-to-terraform
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1726941976901/2115f814-2a02-4293-a3fe-242063aeae29.png
tags: software-development, java, javascript, backend, system-design, terraform

---

This article is for complete beginners and covers all the basics to get started, along with some practical examples and essentials related to Terraform. We will start with the knowledge of Infrastructure as Code. Then, we will understand the need for IaC with Terraform and learn Terraform concepts. Finally, we will perform some practical projects.

## Infrastructure as Code(IaC)

Before diving into Terraform, we should understand what **Infrastructure as Code** is.

Infrastructure as Code (IaC) lets you manage infrastructure with configuration files, defining the "desired state" of your infrastructure. This ensures consistency across environments, automates processes to save time and effort, and allows for version control. IaC tools include cloud-specific options like CloudFormation and Cloud Deployment Manager for single cloud providers, and cloud-agnostic options like Resource Manager Templates and Blueprints for multiple cloud providers. HashiCorp Terraform is a popular IaC tool that allows you to define cloud and on-premises resources in human-readable configuration files using HCL (HashiCorp Configuration Language).

**Terraform** is an infrastructure as code tool that lets you define both cloud and on-premises resources in human-readable configuration files. Isn't it fascinating that you can version, reuse, and share these files?

**Why do we need IAC?**

So, we know that huge microservices run on cloud services like AWS, GCP, etc., and these microservices might number in the thousands if the company is like Google or Facebook. Now, if there are only a few microservices, they might be easy to manage and configure manually. If issues arise due to compatibility or other points of failure, they can be fixed manually. But if the system contains thousands of microservices, it becomes very difficult, if not impossible, to fix the configuration or any other anomaly manually.

This is where the need for Infrastructure as Code (IaC) comes in. IaC allows you to apply code to every microservice, defining certain rules and the desired state of the system that the entire system must fulfill. IaC defines the desired state of the entire system, including configurations, replicas, etc., and always strives to reach that desired state. So if something goes wrong, the system will automatically fix the issue, like recreating a crashed microservice, because it is programmed to maintain the desired state.

**What do we mean by "Infrastructure"?**

Everything that **supports** the application/service to run, including servers, network configurations, storage, and monitoring.

Types - **Physical**, **On Cloud**, Hybrid, Edge Infrastructure

**Physical or On-premises:** Any changes needed were made manually and worked fine because changes were rare, but problems include not being able to scale based on demand, high maintenance costs, and low flexibility, making it challenging to reconfigure.

**Managing your Infrastructure - in Cloud Environments** has led to API-driven components with low manual effort, on-demand scalability, less maintenance time, easy customization, cost efficiency, and simple deployment and management across regions, but it also introduces problems like manual effort, configuration drift, and complex scaling, which can be solved by defining the infrastructure as code.

**Infrastructure as Code (IaC)** lets you manage infrastructure with configuration files, defining the "desired state" of your infrastructure, ensuring consistency across environments, automating processes to save time and effort, and allowing for version control.

**Infrastructure as Code (IaC)**

Tools to provision your infrastructure as code include **Cloud Specific** options like CloudFormation, Cloud Deployment Manager, and Elastic Beanstalk for a single cloud provider, and **Cloud Agnostic** options like Resource Manager Templates and Blueprints for multiple cloud providers.

**Infrastructure as Code Using Terraform**

HashiCorp Terraform is an **infrastructure as code tool** that allows you to define **cloud and on-prem resources** in **human-readable configuration files** that you can **version**, **reuse**, and **share**, using **HCL (HashiCorp Configuration Language)**, following a **declarative approach**, automating the infrastructure lifecycle management, and supporting **version control** and reusability.

**How does Terraform work?**

So basically, there are two states in Terraform. One is the current state, and the other is the desired state. Its main purpose is to maintain the desired state from the current state.

So the question arises, how does Terraform work? terraform uses the Hashicorp copnfiguration language which is kind of json and yaml mix lanhguage easy to understand, manages all the imnfrastrucutre. It has all

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726949250637/d3d2b7cb-1a1f-47fb-8ed6-0c95537bc319.png align="left")

## **Core Concepts**

* Providers
    
* Resources
    
* HCL Language features
    
* State management
    
* Variables and Outputs
    
* Modules
    

**Installation**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726948924042/e5f686e1-efb9-4ddc-bb37-a05eb48ab2f5.png align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726948940181/ea785fc3-3c9e-462c-af5e-cb8e7776e00c.png align="left")

### Provider

A **plugn** used to nteract wth APIs.

Where do they come from?

**\- Terraform Regstry**

### **Resources**

Defines the **ACTUAL components** of the infrastructure. **Syntax:**

Meta-Arguments

**Special arguments** that can be used wth every resource.

**List:**

* **depends\_on**
    
* **count**
    
* **for\_each**
    
* **provider**
    
* **lifecycle**
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726942041334/b200d09b-2078-4871-8970-dcce5011b970.png align="center")