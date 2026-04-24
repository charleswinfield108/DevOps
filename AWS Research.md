# AWS Research — Getting Started with DevOps on AWS

The following three courses are selected from the list **"Getting Started with DevOps on AWS - 11 Best Courses"**. Each summary describes the knowledge and skills the course provides.

---

## 1. Introduction to DevOps and Site Reliability Engineering (LFS162x)

**Provider:** The Linux Foundation via edX  
**Level:** Beginner | **Duration:** ~12 hours | **Format:** Self-paced, free  
**URL:** https://www.edx.org/learn/devops/the-linux-foundation-introduction-to-devops-and-site-reliability-engineering

### What this course covers

This course provides a broad foundation in both DevOps culture and Site Reliability Engineering (SRE). It is structured across the following chapters:

- **Introduction to DevOps and SRE** — How DevOps principles influence modern software delivery and how SRE extends those principles into operations
- **Introduction to Cloud** — How cloud computing enables organisations to rapidly build, deploy, and scale products
- **Introduction to Containers** — The open container ecosystem and how Kubernetes is reshaping software delivery
- **Infrastructure as Code (IaC)** — The why, what, and how of defining infrastructure through code
- **Continuous Integration / Continuous Delivery (CI/CD)** — Pipelines, automation, and safe, repeatable deployment practices
- **Introduction to Observability** — What to monitor, why, and how observability systems support reliable systems
- **Site Reliability Engineering** — SRE culture, error budgets, SLOs, and the operational mindset

### Knowledge gained

Learners leave with a solid, practical understanding of DevOps and SRE fundamentals — enough to deploy software with confidence, agility, and high reliability. The course also includes a discussion forum for peer and expert interaction.

---

## 2. Getting Started with DevOps on AWS

**Provider:** Amazon Web Services via AWS Skill Builder  
**Level:** Beginner | **Duration:** ~60 minutes | **Format:** Free digital course  
**URL:** https://skillbuilder.aws/learn/R4B13K95YQ/getting-started-with-devops-on-aws/38NHHYRV1R

### What this course covers

This introductory course is aimed at technical learners in development and operations roles who want to understand how DevOps works within the AWS ecosystem. Topics include:

- **DevOps culture and philosophy** — The mindset shifts required for teams to adopt DevOps practices effectively
- **Core DevOps practices** — Continuous integration, continuous delivery, microservices, infrastructure as code, monitoring, and logging
- **AWS DevOps tools** — An overview of the AWS-native services that support each DevOps practice (e.g. CodePipeline, CodeBuild, CodeDeploy, CloudFormation)
- **Secure, high-velocity delivery** — Concepts for developing and releasing applications securely at high speed on AWS

### Knowledge gained

By the end of this course, learners can describe how AWS helps teams implement DevOps practices to build and deploy applications more securely, reduce time-to-market, and manage risk. It is an ideal starting point before exploring deeper AWS DevOps tooling and certifications.

---

## 3. Cloud DevOps Engineer Professional Certificate (SRE & DevOps on Google Cloud)

**Provider:** Google Cloud via Coursera  
**Level:** Intermediate | **Duration:** ~5 courses (~4 weeks each at 10 hrs/week) | **Format:** Professional Certificate  
**URL:** https://www.coursera.org/professional-certificates/sre-devops-engineer-google-cloud

### What this course covers

This five-course professional certificate programme teaches the skills required to work as a Cloud DevOps Engineer using Google Cloud Platform. Key topic areas include:

- **SRE culture and DevOps philosophy** — Understanding the principles behind Site Reliability Engineering and how they align with DevOps
- **Cloud infrastructure and networking** — Designing reliable cloud environments, microservices architecture, and cloud networking fundamentals
- **Kubernetes and containerisation** — Orchestrating containerised applications at scale with Google Kubernetes Engine (GKE)
- **CI/CD pipelines** — Building and managing automated deployment pipelines for continuous integration and delivery
- **Observability and incident response** — Implementing monitoring dashboards, logging, alerting strategies, SLOs, SLIs, and KPIs for data-driven decisions
- **Google Cloud DevOps tools** — Hands-on experience with GCP-native services through integrated Qwiklabs practical exercises

### Knowledge gained

Graduates are equipped to design reliable cloud infrastructure, build deployment pipelines, manage containerised workloads, and implement comprehensive monitoring and incident management processes. The certificate also prepares learners for the **Google Cloud Professional Cloud DevOps Engineer** certification exam.

---

## AWS CLI vs AWS Console — When and Why to Use Each

AWS provides two primary ways to interact with its services: the **AWS Management Console** (a web-based graphical interface) and the **AWS CLI** (a command-line tool). Both achieve the same outcomes, but each suits different situations.

### The AWS Management Console

The Console is a browser-based dashboard where you can point and click to configure, monitor, and manage AWS services without writing any commands.

**Best used when:**

- **Learning or exploring AWS** — The visual interface makes it easy to discover services, read descriptions, and understand what options exist without prior knowledge of command syntax
- **One-off or infrequent tasks** — For example, setting up a new IAM user, checking billing, or viewing CloudWatch logs once in a while
- **Visualising infrastructure** — Diagrams, dashboards, and resource maps are only available in the Console (e.g. VPC topology views, CloudFormation stack visualisations)
- **Troubleshooting** — Quickly inspecting the current state of a resource, reading error messages, or comparing settings side by side is faster in a GUI
- **Non-technical stakeholders** — Team members who need read-only visibility into infrastructure without learning CLI commands

### The AWS CLI

The CLI is a terminal-based tool that lets developers interact with AWS services by running commands, making it scriptable and automatable.

**Best used when:**

- **Automation and scripting** — CLI commands can be embedded in shell scripts, CI/CD pipelines, or Makefiles to perform repeatable tasks without manual steps (e.g. deploying a Lambda function on every code push)
- **Infrastructure as Code workflows** — CLI commands integrate with tools like Terraform, Ansible, and AWS CloudFormation to provision and manage resources programmatically
- **Bulk operations** — Tasks that would require clicking through dozens of pages in the Console (e.g. deleting 100 S3 objects, updating security groups across multiple regions) can be done in a single command or loop
- **Remote and headless environments** — CI servers, Docker containers, and EC2 instances have no browser; the CLI is the only option in these environments
- **Speed and precision** — Experienced developers can execute complex operations faster by typing a command than by navigating through multiple Console menus
- **Auditability and reproducibility** — CLI commands can be saved, version-controlled, and shared with a team, making infrastructure changes traceable and repeatable

### Summary

| | AWS Console | AWS CLI |
|---|---|---|
| Interface | Web browser (GUI) | Terminal (text commands) |
| Best for | Exploration, visualisation, one-off tasks | Automation, scripting, CI/CD, bulk operations |
| Reproducible | No — manual clicks leave no record | Yes — commands can be scripted and version-controlled |
| Requires internet browser | Yes | No |
| Learning curve | Low | Moderate |

In a DevOps context, the CLI is the preferred tool for day-to-day work because it supports the core DevOps principle of **automating everything**. The Console remains valuable for initial setup, monitoring, and any task where visual context is more important than speed.

---

*Research compiled: April 2026*
