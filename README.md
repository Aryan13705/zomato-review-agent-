

# 📊 Google Play Store Review Trend Analysis AI Agent

## Overview

This project implements an **agentic AI system** to analyze Google Play Store reviews for the **Zomato** app and generate a **rolling 30-day trend analysis** of user issues.

The system ingests reviews, extracts normalized issue topics, aggregates them day-wise, and produces a **CSV trend table** that helps product teams identify **recurring and rising problems** over time.

---

## Problem Statement

User reviews contain valuable feedback, but:

* The same issue is described in different ways
* Similar complaints get fragmented
* Manual trend tracking is inefficient

The objective is to **automatically extract, normalize, and track review topics over time**.

---

## Key Features

* ✅ Automated Google Play Store review ingestion
* ✅ Semantic topic normalization (issue deduplication)
* ✅ Rolling T-30 → T trend analysis
* ✅ Daily topic frequency aggregation
* ✅ Topic confidence scoring
* ✅ CSV output suitable for analytics and reporting

---

## System Architecture (Agentic Design)

```
Review Fetching Agent
        ↓
Topic Extraction Agent
        ↓
Topic Normalization & Deduplication
        ↓
Trend Aggregation Agent
        ↓
CSV Trend Report
```

Each agent performs a **single responsibility**, making the system modular and extensible.

---

## Topic Extraction Strategy (Hybrid & Robust)

### LLM-Ready Design

The topic extraction module is designed to support **LLM-based semantic extraction**, enabling:

* Meaning-based grouping
* High recall
* Dynamic topic discovery

### Hybrid Fallback Strategy

During development, external LLM APIs (OpenAI / Gemini) were constrained by **quota and model availability**.

To ensure **reliable and reproducible execution**, a **hybrid fallback strategy** was implemented:

* When LLM access is unavailable, the system automatically falls back to a **semantic rule-based extractor**
* The agentic pipeline remains unchanged

This mirrors **real-world production systems**, where graceful degradation is essential.

---

## AI Validation & Quality Assurance

### 1. Manual Review Verification

Sample reviews were manually compared with extracted topics.

**Example**

Review:

> “The delivery partner was rude and food arrived cold.”

Extracted Topics:

* delivery partner rude
* food quality issue

This confirms semantic correctness.

---

### 2. Semantic Consistency

Different phrasings such as:

* “delivery guy behaved badly”
* “rider was rude”

are normalized into a **single topic**, preventing fragmented trends.

---

### 3. Temporal Consistency

Daily aggregation was validated to ensure:

* Correct date mapping
* Accurate rolling 30-day window
* No cross-day leakage

---

## Confidence Scoring

Each topic is assigned a **confidence score** based on its relative frequency across the analysis window.

* Frequently recurring topics → Higher confidence
* Rare or noisy topics → Lower confidence

This helps prioritize **reliable signals** over isolated complaints.

---

## Sample Output (Trend Table)

**Rows → Topics**
**Columns → Dates (T-30 to T)**
**Values → Frequency of topic occurrence**

```csv
topic,2024-06-01,2024-06-02,2024-06-03,2024-06-04,2024-06-05,2024-06-06,2024-06-07,2024-06-08,2024-06-09,2024-06-10,2024-06-11,2024-06-12,2024-06-13,2024-06-14,2024-06-15,2024-06-16,2024-06-17,2024-06-18,2024-06-19,2024-06-20,2024-06-21,2024-06-22,2024-06-23,2024-06-24,2024-06-25,2024-06-26,2024-06-27,2024-06-28,2024-06-29,2024-06-30
delivery delay,12,14,15,16,18,20,22,19,21,23,24,26,25,27,28,30,31,29,32,33,34,35,36,34,37,38,39,40,41,42
delivery partner rude,3,4,5,5,6,7,8,7,8,9,9,10,11,10,12,13,12,13,14,15,16,17,18,17,18,19,20,21,22,23
food quality issue,8,9,10,11,12,13,14,13,14,15,16,17,18,17,19,20,21,20,22,23,24,25,26,25,27,28,29,30,31,32
refund issue,2,3,3,4,4,5,6,5,6,6,7,7,8,7,8,9,9,10,10,11,11,12,13,12,13,14,14,15,16,16
customer support issue,5,6,7,7,8,9,10,9,10,11,12,12,13,14,15,15,16,17,18,19,20,21,22,21,23,24,25,26,27,28
```

---

## Project Structure

```
zomato-review-agent/
│
├── fetch_reviews.py
├── llm_topic_extractor.py
├── build_trend_table.py
│
├── data/
│   ├── raw_reviews.json
│   └── topic_reviews.json
│
├── output/
│   ├── trend_report.csv
│   └── topic_confidence.csv
│
└── README.md
```

---

## How to Run

```bash
python -m venv venv
venv\Scripts\activate
pip install google-play-scraper pandas

python fetch_reviews.py
python llm_topic_extractor.py
python build_trend_table.py
```

---

## Why This Approach

* ❌ Avoids traditional topic modeling (LDA / TopicBERT)
* ✅ Focuses on semantic issue grouping
* ✅ Produces interpretable, time-aware insights
* ✅ Designed for robustness and extensibility



---

## Conclusion

This project demonstrates a **robust, production-oriented AI system** for review trend analysis.
It emphasizes **semantic consistency, reliability, and explainability**, making it suitable for real-world product analytics.

