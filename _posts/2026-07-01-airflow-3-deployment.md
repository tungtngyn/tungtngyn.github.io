---
title: "Airflow 3 Distributed Deployment"
excerpt: "Architected and deployed a distributed Apache Airflow 3 cluster on AWS, establishing a centralized orchestration engine for my team's workflows."
tags: [airflow, aws, terraform, postgres, docker, celery, sqs, s3, ecs, lambda, git]
---

Association(s): Collins Aerospace (RTX)

# Overview

My team at Collins Aerospace has used Airflow since the team's inception. Our initial deployment, which I did not design, followed a relatively monolithic architecture: each Airflow component ran in its own container, but all containers shared the same compute resources as they were all deployed in the same AWS ECS Task.

The deployment was originally intended as an R&D project but eventually became the foundation for production workflows as infrastructure resources were redirected elsewhere. Over time, this resulted in significant technical debt. The architecture was sufficient while we had a small number of pipelines, but began to show limitations as more workflows and engineers were onboarded:

* Inexperienced engineers could push bad code directly to Development, where code reviews were not required, causing the environment to crash.
* The production cluster required increasingly large amounts of compute to support growing workloads, driving up costs.
* A lack of standardized utilities led to multiple overlapping implementations of common functionality.
* The deployment ran on Python 3.7, which reached end-of-life in ~2023 and created a compliance issue.

I highlighted these issues to my leadership and proposed a new distributed architecture, which was approved and became the basis for a complete redesign. My primary focus was on improving the developer experience, reducing infrastructure costs, and addressing all of the team's accumulated technical debt.

# System Architecture

![Airflow Architecture Diagram]({{ '/assets/images/AirflowArchitecture.jpg' | relative_url }})

As part of the redesign, I prioritized reliability, scalability, and operational efficiency. Key architectural decisions included:

* **Fully distributed architecture:** Each Airflow component runs in its own AWS ECS Service, allowing components to be independently managed and scaled.
* **Airflow 3.x upgrade:** Migrated from Airflow 2.x to Airflow 3.x, resolving the Python 3.7 compliance issue.
* **Independent worker scaling:** Worker capacity scales independently based on workload demand. Previously, compute was provisioned for peak usage, leaving a large cluster idle for several hours each day and driving unnecessary costs.
* **Automated CI/CD:** Implemented Git Bundle-based DAG deployment with a Lambda function that automatically refreshes access tokens, eliminating manual DAG refreshes.
* **Multiple executors:** Introduced `CeleryExecutor` for general-purpose, relatively lightweight workloads such as database operations, and `AwsEcsExecutor` for CPU-intensive or memory-intensive workloads such as transferring large datasets between databases.
* **Fault-tolerant execution model:** Replaced `LocalExecutor` with `CeleryExecutor` backed by AWS SQS, enabling distributed task execution and isolating worker failures from the core Airflow services.
* **S3-backed XCom:** Moved XCom storage from PostgreSQL to AWS S3 with automated retention policies, preventing XCom data from accumulating in the Airflow metadata database.
* **Standardized utilities and dbt integration:** Consolidated common functionality into standardized utilities and introduced **dbt** to provide a consistent syntax and framework for data transformations.
* **Secrets manager integration:** Migrated Airflow Connections and Variables to AWS Secrets Manager, establishing a centralized source of truth for credentials and configuration values. Previously, the team maintained secrets in both AWS Secrets Manager and Airflow's metadata database, creating duplication and additional maintenance overhead.

<br/>

# Future Opportunities

While the new architecture addresses many of the platform's existing limitations, some opportunities I see for the future of this Airflow deployment are:

* **Standardized unit testing framework:** Develop a common testing framework for Airflow DAGs, operators, and utilities to catch issues earlier in the development lifecycle and improve confidence when deploying new workflows.
* **AI-assisted migrations:** Develop AI-powered migration tools to streamline onboarding additional teams onto the shared Airflow platform, automate legacy DAG migrations, and reduce the effort required to modernize existing deployments.
* **Enhanced observability and lineage:** Expand platform monitoring and data lineage capabilities to provide visibility into pipeline execution patterns, including which workflows are running, when they execute, their dependencies, and which pipelines consume the most compute resources. This would enable more proactive optimization of performance, reliability, and infrastructure costs.
