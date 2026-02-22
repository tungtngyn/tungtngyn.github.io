---
title: "Automated Contractual Assertions System"
excerpt: "An integrated ecosystem of data tools used to automatically identify $100M/yr in assertion opportunities."
tags: [airflow, snowflake, SAP, business-intelligence]
---

Association(s): RTX

# Overview

Many contracts at RTX include assertion clauses that apply when a vendor underperforms, typically measured by an On-Time Delivery (OTD) metric. At its core, OTD is the ratio of on-time deliveries to total deliveries, with additional penalties being applied for overdue parts. However, in practice, pursuing assertions can be complex. Receiving data is often difficult to reconcile with the vendor's shipping data, legal documentation requirements are extensive, and damage calculations may involve intricate business logic such as tiered penalties, foreign currency considerations, and multiple (often international) factory calendars.

To streamline this process, I developed an ecosystem of data tools to centralize and automate contractual assertions, including:
* A simple Contract Metadata Database built using **SQL Server** & **Excel VBA**
* **Airflow** pipelines that pull receiving data, apply custom pegging logic, and outputs report-ready assertion tables
* An **Airflow** pipeline which generates legal documentation on-demand, triggered by users via the Airflow REST API
* **Tableau** dashboards to identify underperforming vendors & track data loads

In its first year of operation, the system identified over $100M in assertion opportunities, with approximately $15M successfully collected. Due to its success, senior leadership identified this tool as a strategic opportunity for enterprise-scale adoption.

<br/>

# System Architecture

![Contractual Assertions System Architecture Diagram]({{ '/assets/images/AssertionsArchitecture.jpg' | relative_url }})

The core differentiator of this system is not technical in nature - rather, its innovation lies in reframing a manual, fragmented business process as a data product problem. By centralizing and standardizing contractual metadata into a common data model, this tool automates work that otherwise would be economically impractical (i.e. having a lawyer spend multiple hours drafting documentation for assertions <$100K in value). As the system scales, these small assertions compound over time and end up having a meaningful financial impact.

Two Airflow pipelines power the solution: the first applies custom business logic to reconcile vendor invoice and shipping data with RTX receiving data, producing assertion-ready datasets. The second dynamically computes damages using contractual metadata and automatically generates the required legal documentation.

<br/>

# Future Opportunities

This system started off as an automated performance assertion engine, but has significant opportunities to expand beyond just assertions. Some ideas I proposed were:

* **Enterprise CLM Platform**: Centralize contract PDFs and metadata into a unified lifecycle management system, enabling version control, searchability, and auditability.
* **ML-Based Metadata Extraction**: Leverage OCR and NLP technologies to automatically extract contractual terms from PDFs, reducing manual work and increasing scalability.
* **Expanded Assertion Coverage**: Extend the framework to adjacent clauses, such as quality guarantees (i.e., part is delivered on time but there are defects).
