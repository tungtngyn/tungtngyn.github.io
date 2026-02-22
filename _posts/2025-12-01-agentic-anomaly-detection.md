---
title: "Agentic Anomaly Detection System"
excerpt: "An LLM-powered anomaly detection system for multi-variate time series data."
tags: [anomaly-detection, llm, chatbot]
---

Association(s): Georgia Tech

# Overview

Built an end-to-end anomaly detection system for multivariate time series data by combining time-series forecasting algorithms with a language-model-driven explanation layer. The goal was to develop a proof-of-concept industrial application that is accurate, interpretable, and low-cost (at time of inference). 

Using the [MetroPT Dataset](https://zenodo.org/records/6854240), a total of 7 anomaly detection models were evaluated; 3 of which were [Prophet](https://facebook.github.io/prophet) models that were adapted for anomaly detection. The best performing model was integrated into a webapp built in [Streamlit](https://streamlit.io) where users could interact with model outputs via a chatbot interface. 

The demo video below demonstrates some of the capabilities of the system such as: 
* Data retrieval & aggregation
* Interpretation of outputs from the anomaly detection model
* Context retrieval (RAG)
* Plotting of flagged anomalies

If you'd like to do a deep-dive on this project, read the report and code [here](https://github.com/tungtngyn/practicum/tree/main).

<iframe width="560" height="315" src="https://www.youtube.com/embed/GOR1r17o70s?si=u1iFfwxyMe52CPEG" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<br/>

# System Architecture

![Agentic Anomaly Architecture Diagram]({{ '/assets/images/AgenticAnomalyArchitecture.jpg' | relative_url }})

The system is fully built in Python and has a relatively simple data flow:
* Raw data is fed into a **SQLite** database
* Documentation is fed into a [Chroma](https://www.trychroma.com/) vector database
* Multiple **Prophet** models are trained; 1 for each sensor signal. 
* Model outputs are written back to the database on a regular cadence - i.e., batch inference (hourly, daily, weekly, etc)
* Streamlit app exposes a chatbot interface to users and is connected to a [LangGraph](https://www.langchain.com/langgraph) flow that orchestrates an LLM to respond to users
* Retrieval workflow includes a RAG component as well as tools for interacting with the underlying system database

<br/>

# Challenges

The primary challenges stemmed from both the dataset and the nature of predictive maintenance problems. The training data was only partially supervised, with just three major failures recorded and the possibility of additional, less severe issues that were never labeled. The anomaly detection model also needed to identify failures *before* they occurred, rather than during the failure period itself. This constraint made traditional algorithms difficult to apply, since the model had to recognize subtle, early deviations instead of obvious failure states.

To address the issues above, two methods were proposed: 
* A pseudo-label approach which rewarded models for flagging time periods leading up to a major failure
* An automated model tuning framework which trained models using randomly generated synthetic anomalies

The pseudo-label approach was fully implemented and was used to train the aforementioned 7 models. The automated tuning framework was only implemented as a proof-of-concept due to computational constraints. For full details, read the formal report in the GitHub repository linked above!

<br/>

# Future Opportunities

While the proposed system met its primary objectives, there's always room for improvement. Some future areas of research that I'd want to investigate are:

* **Online Learning:** Extend the current Prophet-based algorithm to work with real-time data instead of batch inference.
* **Agentic Layouts:** Provide the LLM the capability to change the webapp layout based on the user’s questions – multiple plots, different types of plots, interactive charts, etc.
* **Different Anomaly Types:** Extend the proposed automated model tuning framework to handle different types of anomalies, ex. a change in frequency or trend could also be anomalous for certain systems.
