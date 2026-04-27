# Big Data Lab 02: Spark Structured Streaming Analysis

## 1. Project Overview
This project implements a real-time analytics engine using **Spark Structured Streaming** — a scalable and fault-tolerant engine built on Spark SQL.

The system:
- Ingests movie-related data from **Apache Kafka**
- Performs complex transformations
- Delivers near real-time insights into:
  - Movie trends
  - Popular genres
  - User interactions

---

## 2. Learning Objectives

- Master Structured Streaming APIs for incremental micro-batch execution  
- Implement **Event-time processing** using Watermarks to handle late-arriving data  
- Apply Windowing strategies:
  - Tumbling Window  
  - Hopping Window  
  - Sliding Window  
- Understand execution modes:
  - Append  
  - Update  
  - Complete  

---

## 3. System Architecture

The pipeline consists of three main stages:

### 1. Data Source (Producer)
- A Python script simulates real-time streaming
- Pushes JSON records into Kafka topics:
  - `Lab1_movies`
  - `Lab1_ratings`
  - `Lab1_tags`

### 2. Processing Engine (Spark)

- **Exercise 1:** Schema enforcement & data cleaning  
- **Exercise 2:** Stream-Static Join → Identify "hot" genres  
- **Exercise 3:** Windowed Top-K analysis (5-minute Tumbling Window)  
- **Exercise 4:** Stream-Stream Join → Correlate ratings & tags  

### 3. Data Sink
- Results displayed in **console**
- Trigger interval: **5 seconds**

---

## 4. Implementation Details

### Running the Project

#### 1. Start Kafka
- Ensure:
  - Zookeeper is running
  - Kafka brokers are active

---

#### 2. Producer Setup
- Run `Lab02_Producer.ipynb`
- Behavior:
  - Pushes **100 rows per topic every 2 seconds**
  - Keeps streams synchronized for joins

---

#### 3. Consumer Execution

- Open `Lab02_Consumer.ipynb`
- Configure SparkSession:
  - Use unique UI port (e.g., `4041`)
  - Avoid conflict with Producer

- Run each exercise
- Cause the data is large, you can enterupt the process when you can see the output for demo
- After run excercise, please rung the `kill query` to kill the existing query, and then run the others

---

### Exercise 1: Preparation
- Define schemas for:
  - Movies
  - Ratings
  - Tags
- Convert Kafka JSON strings → Structured DataFrames
- Apply proper data types

---

### Exercise 2: Hot Genres

**Logic:**
- Join streaming ratings with static movies dataset

**Technique:**
- Use `explode()` to flatten genres
- Aggregate count per genre

---

### Exercise 3: Trending Now (Top-K)

**Windowing:**
- 5-minute **Tumbling Window** on event time

**Watermarking:**
- Applied on ratings stream
- Handles late-arriving data

**Ranking:**
- Use `dense_rank()` over:
  - Partition: time window  
  - Order:
    - Count (descending)
    - movieId (ascending for tie-break)

---

### Exercise 4: Stream-Stream Join

**Logic:**
- Join real-time:
  - Ratings stream
  - Tags stream

**Constraints:**
- Requires Watermarks on both streams
- Join condition: `movieId`

**Mode:**
- Operates in **APPEND mode**
- Outputs only matching records

---

## 5. Usage Instructions

### Prerequisites

- Apache Spark **4.0.1+**
- Apache Kafka **3.6.0**
- Python **3.12+**

---


 