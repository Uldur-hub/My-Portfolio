---
layout: post
title: "Cyclistic Data Analysis Case Study"
author:
- Hugo Escobedo
---

![Banner](/assets/cyclistic-logo.png)

<br> 

# Table of Contents
- [Introduction](#introduction)
- [Scenario](#scenario)
- [Deliverables](#deliverables)
<br>

1. **[Ask Phase (Business Task Statement)](#ask-phase)**
- [Problem to Solve](#problem-to-solve)
- [Question to answer](#question-to-answer)
- [Business Task Statement](#business-task-statement)
- [Key Stakeholders](#key-stakeholders)

# Introduction

Welcome to my Google Data Analytics Case Study.
My name is Hugo and in this project, I will take on the role of a Junior Data Analyst on the marketing analytics team at a fictional bike-share company based in Chicago. The objective of this case study is to analyze historical trip data to identify behavioral differences between casual riders and annual members. These insights will inform the development of a targeted marketing strategy aimed at converting casual riders into annual members, thereby increasing the company's overall revenue.

The analysis will be conducted using SQL, and Tableau, and will follow Google’s six-step data analysis process: ***Ask, Prepare, Process, Analyze, Share, and Act.***

# Scenario

You are a junior data analyst working on the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.

# Deliverables

1. A clear statement of the business task.
2. A description of all data sources used.
3. Documentation of any cleaning or manipulation of data.
4. A summary of the analysis.
5. Supporting visualizations and key findings.
6. Top three recommendations based on the analysis.

# Ask Phase

In the first phase of the analysis my aim is to fully understand the stakeholders expectations, define the problem to be solved and decide which questions to answer in order to solve the problem.

#### Problem to Solve

- Convert casual riders into annual riders

#### Question to answer

- How do annual members and casual riders use Cyclistic bikes differently?

#### Business Task Statement

***Identify key usage patterns and behavioral differences between casual riders and annual members*** of Cyclistic bike-share services, in order to ***develop targeted marketing strategies*** that effectively ***convert more casual riders into annual members***, thereby increasing overall revenue and customer retention.

#### Key Stakeholders

- ***Cyclistic Executive Team:*** Responsible for approving the recommended marketing program.
- ***Lily Moreno:*** Director of Marketing, responsible for the development of campaigns and initiatives to promote the bike-share program.

[Back to Top](#table-of-contents)

# Prepare Phase (Data Sources)

#### Key Focus Areas in the Data Preparation Phase:

- **Identify the Data:** Determine what data is required to support the analysis.
- **Locate the Data:** Identify where the data is stored and how it can be accessed.
- **Understand Data Structure:** Examine how the data is organized and formatted.
- **Assess for Bias and ROCCC:** Evaluate the data for potential bias and ensure it meets the ROCCC criteria—Reliable, Original, Comprehensive, Current, and Cited.
- **Address Legal and Ethical Considerations:** Review licensing, privacy, security, and accessibility requirements related to the data.
- **Ensure Data Integrity:** Verify the accuracy, consistency, and reliability of the data.
- **Identify Data Issues:** Detect any errors, anomalies, or quality concerns that may affect the analysis.

#### Data Source

Cyclistic’s first-party 2024 historical trip data is stored [here](https://divvy-tripdata.s3.amazonaws.com/index.html). The datasets have a different name because Cyclistic is a fictional company. For the purposes of this case study, the datasets are appropriate and will enable me to answer the business questions. The data has been made available by Motivate International Inc. under [this license](https://divvybikes.com/data-license-agreement).

#### Data Overview and Quality Summary

The analysis is based on Cyclistic’s 2024 historical trip data, provided in 12 structured `.CSV` files organized by rows and columns. Given the record limitations inherent to spreadsheet software, it was not feasible to efficiently execute the processing and analysis phases within those platforms. As a result, the entire workflow was conducted using BigQuery SQL for data processing and Tableau for data visualization.

Although Cyclistic is a fictional company, the data—made available by Motivate International Inc. under an open license—is appropriate for the scope of this case study and supports the business objectives. The data does not present any major or critical issues. Minor issues such as NULL values have been identified and will be addressed during the data cleaning process to ensure integrity and accuracy in analysis.

#### ROCCC Data Compliance Assessment:

- **Reliable:** The dataset originates from Cyclistic’s internal record systems and consists of historical trip data, minimizing bias as it reflects actual user behavior.
- **Original:** The data has been directly validated through Cyclistic’s proprietary data collection processes, ensuring authenticity.
- **Comprehensive:** It includes detailed trip information for both casual riders and annual members throughout the year 2024, with each row representing an individual trip.
- **Current:** The dataset is up-to-date, covering the year 2024, which aligns with the timeframe of this analysis.
- **Cited:** The source of the data has been properly referenced for transparency and traceability in the _**Data Source**_ segment.
