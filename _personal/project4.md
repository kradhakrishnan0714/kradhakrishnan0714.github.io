---
layout: single
title: "Performance comparison between delta lake and traditional data lake"
classes: wide
date: 2025-03-08
---

**Description**: Traditional data lakes often face challenges such as data inconsistency, slow query performance, and the absence of transactional guarantees, leading to inefficiencies in data management and analysis, particularly with streaming and large-scale datasets.

Delta Lake, an open-source ACID table storage layer over cloud object stores, uses a transaction log that is compacted into Apache Parquet format to provide ACID properties, time travel, and significantly faster metadata operations for large tabular datasets (e.g., the ability to quickly search billions of table partitions for those relevant to a query).

In this paper, we investigate the performance impact of delta lake features compared to traditional data lake.  

**Tech Stack**: Databricks, Python, SQL

## Analysis & Performance results

<iframe 
  src="/assets/docs/Delta lake performance evaluation.pdf" 
  width="100%" 
  height="600px" 
  style="border: none;">
  This browser does not support PDFs. 
  <a href="/assets/docs/Predicting success of marketing campaigns.pdf">Download PDF</a>
</iframe>