---
title: "Statistical Simulation Webapp for Contractual Escalation Clause"
excerpt: "A webapp used to evaluate escalation clauses and forecast part pricing based on economic data and contractual terms."
tags: [statistics, aws, python, flask]
---

Association(s): RTX

# Overview

In the Aerospace & Defense industry, a single aircraft be in production for more than 30 years. Under traditional Firm-Fixed-Price (FFP) contracts, contractors were responsible for absorbing inflation-related cost increases over the life of the program. However, rising inflation in recent years has led to wider adoption of Economic Price Adjustment (EPA) clauses, which allow for annual price increases based on material and labor indices.

To support this paradigm shift, I partnered with commodity and contract managers to build a web application that uses statistical simulations to model how part prices may fluctuate over the life of a program, enabling more informed sourcing decisions.

This tool ingests economic data from sources such as the Bureau of Labor Statistics, applies forecasted data from S&P Global, and enables users to model complex EPA formulas. These models support weighted combinations of multiple indices, escalation caps, vendor–customer escalation splits, and other contract-specific rules.

I did the full-stack development for this app, which included:
* Building **Airflow** pipelines to extract the latest economic and forecast data daily
* Developing a **Python**-based (**Flask**) app with Single-Sign-On integrated
* Deploying a containerized app on **AWS ECS**

<br/>

# System Architecture

![EPA Tool Architecture Diagram]({{ '/assets/images/EPAArchitecture.jpg' | relative_url }})

The system uses a lightweight Single Page Application (SPA) architecture with **Flask** as the primary web framework (SSO extensions added for authentication). Interactive charts were fully built in **Bokeh**, running as a separate process in the same container, and integrated into the Flask UI via a WebSocket connection. Business and statistical sampling logic is primarily implemented in **numpy**. 

<br/>

# Future Opportunities

This system still has significant opportunities for expansion. As EPA clauses become more common, new contracts introduce increasingly diverse and complex terms to adapt to the many different product portfolios at RTX, requiring ongoing extensions to the application's business logic. There are also opportunities to integrate this tool with existing enterprise systems, such as contract lifecycle management (CLM) tools for company-wide EPA clause tracking and analysis, and project and portfolio management (PPM) systems to automate quarterly part price forecasts.
