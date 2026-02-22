---
title: "Line Disruption Webapp: Streamlining Supply Chain Collaboration"
excerpt: "A data-intensive webapp used to manage material shortages."
tags: [airflow, aws, terraform, snowflake, postgres, docker, python, fastapi]
---

Association(s): RTX

# Overview

The Line Disruption Tool (LDT) is an internal, cloud-native application developed for the Supply Chain organization at RTX. It's a data-intensive, web-based platform that consolidates information from ERP systems, external APIs, and internal databases. By unifying these disparate sources, the system equips procurement teams with the tooling needed to detect and mitigate material shortages across the entire company.

The tool also promotes cross-functional collaboration through features such as comments, status indicators, escalation workflows, and email notifications. In addition to collaboration tools, the platform offers many customization options, enabling users to save and load personalized configurations across all data grids.

I led backend development for this project, which included:
* Building data pipelines on **Airflow** and **Snowflake**
* Designing schemas, indexing strategies, and query optimizations for a **Postgres** application database
* Developing a **Python**-based **FastAPI** backend
* Deploying a containerized, auto-scaling backend on **AWS ECS**
* Provisioning and managing cloud infrastructure using **Terraform**

<br/>

# System Architecture

![Line Disruption Tool Architecture Diagram]({{ '/assets/images/LineDisruptionArchitecture.jpg' | relative_url }})

Key design features of this platform include: 
* Implementing a Slowly Changing Dimension Type 2 (SCD2) pattern across all core tables to support traceability and root-cause analysis (i.e. in the scenario where there is a process gap).
* Separating the platform into modular components to separate concerns, such as dedicated pages for external parts vs. internal stock transfers.
* Enforcing data governance through Single Sign-On (SSO) and role-baseŒd access controls aligned with the principle of least privilege.
* Centralizing secrets management using AWS Secrets Manager, with automated rotation via Lambda functions.

Most notably, the system is designed with a config-driven development model, meaning a large portion of backend logic and frontend behavior is controlled by configuration files stored on S3. This approach allowed my team to respond quickly to changing business requirements without requiring full application deployments. It also helped us customize the application for different teams across the company, which was an important requirement - RTX purchases a wide variety of parts, and procurement practices can vary drastically!

<br/>

# Future Opportunities

Some ideas I proposed for improvements to this tool included:
* **Documentation Chatbot**: Help new users to onboard faster by integrating a chat interface. The LDT is a complex application and definitely has a learning curve. There's ample documentation available both in-tool and via powerpoints/word docs, but sometimes you just need a quick answer without digging through documentation.
* **Agentic Workflows**: Power-users of the app are constantly flying around, especially if they support procurement for multiple plants. Agentic workflows would allow them to bulk-update and aggregate data across multiple modules without needing to navigate to different pages.
* **Predictive Disruptions**: Using all of the data that the app consolidates, build a machine learning model to *predict* future disruptions.
