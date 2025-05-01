**Social Media Toxicity and Engagement analysis(Sports trends)**

An interactive end-to-end system for crawling, analyzing, and uncovering insights from sports discussions on Reddit and 4chan.

---
## 📋 Table of Contents
1. [Introduction](#introduction)  
2. [Stage 1: Data Collection](#stage-1-data-collection)  
3. [Stage 2: Data Analysis](#stage-2-data-analysis)  
4. [Stage 3: Insights & Findings](#stage-3-insights--findings)  
5. [Interactive Dashboard](#interactive-dashboard)  
6. [Tech Stack](#tech-stack)  
7. [Data Schema](#data-schema)
8. [Contributors](#contributors)
---

## Introduction
In today’s digital age, online sports communities generate mountains of conversations—ranging from play-by-play reactions to heated debates. **Sports Social Media Insights Pipeline** captures this dynamic discourse to answer a pivotal question:

> **Does user engagement correlate with toxicity?**

Through a three-stage process **Data Collection**, **Data analysis**, and **Data inference** we transformed raw comments into actionable insights, empowering researchers and moderators to foster healthier online spaces.

---

## Stage 1: Data Collection
Our robust crawler ingests real-time posts and comments from:

- **Reddit**: Subreddits r/sports, r/Cricket, r/Soccer, r/Football, r/tennis, r/NBA, r/NFL  
- **4chan (/sp/ board)**: Archive endpoint & live threads  

**Key Features**:
- **Producer-Consumer** via Faktory queues  
- **Dockerized** workers scheduled every **5 minutes**  
- **Deduplication & Filtering** at source to reduce noise  

**Total volume of data collected throughout the project timeline**:  
- **Reddit**: ~683K posts (8,128 flagged, 674,962 normal) and ~649K comments across tracked subreddits.
- **4chan**:  ~8.63M posts (199,926 flagged, 8,425,560 normal)
---

## Stage 2: Data Analysis

### Preprocessing & Storage
- **Cleaning**: Remove incomplete records, normalize timestamps, and strip HTML markdown.  
- **Toxicity Scoring**: Used the ModerateHateSpeech API to classify posts and comments (score range: 0–1)  
- **Storage**: PostgreSQL with TimescaleDB extension for efficient time-series querying.

<details>
<summary>Sample Data Transformation(stats based on intial collecton of data)</summary>

| Source  | Raw Count | Cleaned Count |
|---------|-----------|---------------|
| Reddit  | 120,000   | 110,432       |
| 4chan   | 550,000   | 530,115       |

</details>

### Exploratory Analysis
- **Engagement Metrics**: Upvotes, comments per post, shares.  
- **Toxicity Distribution**: 10% flagged posts on Reddit vs. 19% on 4chan.  
- **Time-Series Trends**: Activity spikes during major sports events (e.g., playoffs, finals).

### Sentiment & Engagement Analysis
- Used NLP libraries (NLTK, Hugging Face Transformers) to analyze emotional tone.
- Toxic posts labeled as:
- flag if toxicity > 0.5 (soft threshold)
- True in Toxic_Flag if toxicity > 0.9 (strict classification)

High engagement = Top quartile in upvotes/comments
---

## Stage 3: Insights & Findings

Our investigation confirms a **positive correlation** between engagement and toxicity:

1. **Flagged vs. Normal Posts**  
   - **Reddit**: 8,128 flagged / 674,962 normal  
   - **4chan**: 199,926 flagged / 8,425,560 normal

2. **Engagement Amplifies Toxicity**  
   - Top-quartile posts by engagement scored on average **0.42** toxicity vs. **0.18** for lower quartiles.  
   - Scatter plots reveal clusters of high-engagement, high-toxicity content. 

3. **Platform Differences**  
   - 4chan’s minimal moderation sees nearly **twice** the toxicity rate of Reddit.  
   - Reddit’s diverse subreddit culture yields more nuanced sentiment distribution.

4. **Temporal Insights**  
   - Election season and championship games drive sharp engagement peaks and surges in toxic posts.

> _"Controversial content drives clicks—but at what cost?"_

---

## Interactive Dashboard
Explore a live, web-based interface to query, filter, and visualize:

- **Time-series sentiment flows**  
- **Engagement vs. toxicity scatter plots**  
- **Subreddit and board-specific breakdowns**  
![image](https://github.com/user-attachments/assets/48891be6-5ad2-482f-8f1d-e45071b7717e)
![image](https://github.com/user-attachments/assets/e87c9060-220b-444c-b75e-49a31a2dcd42)
![image](https://github.com/user-attachments/assets/2cd132cf-0d13-4a03-a753-f82d792e14e4)
![image](https://github.com/user-attachments/assets/32a162ad-101c-4e2e-93c7-b4f3ae2b0609)
![image](https://github.com/user-attachments/assets/6c324fb0-a06b-4061-bc29-5d1ee80c367f)

---

## 🛠️ Tech Stack
- **Languages**: Python3 
- **Orchestration**: Docker, Faktory  
- **Data**: PostgreSQL + TimescaleDB  
- **API**: Reddit API, 4chan API, ModerateHatespeech API  
- **Dashboards**: Plotly Dash, Streamlit, Flask

---

## Data Schema
**Reddit Posts**
| Field       | Type    | Description                        |
|-------------|---------|------------------------------------|
| post_id     | VARCHAR | Unique post identifier             |
| author      | VARCHAR | Username                           |
| title       | TEXT    | Post title                         |
| selftext    | TEXT    | Body content                       |
| score       | INT     | Upvotes minus downvotes            |
| num_comments| INT     | Comment count                      |
| created_at  | TIMESTAMPTZ | Posted timestamp                |
| subreddit   | VARCHAR | Source subreddit                   |
| toxicity    | FLOAT   | Toxicity score (0–1)              |

**4chan Posts**
| Field       | Type     | Description                        |
|-------------|----------|------------------------------------|
| post_num    | BIGINT   | Global post identifier             |
| thread_num  | BIGINT   | Thread number                      |
| board       | VARCHAR  | Board name (e.g. `/sp/`)           |
| content     | TEXT     | Post text/images metadata          |
| created_at  | TIMESTAMPTZ | Posted timestamp                |
| toxicity    | FLOAT    | Toxicity score (0–1)              |



## Contributors
- **Saiprakash Nalubolu** ([snalubolu@binghamton.edu](mailto:snalubolu@binghamton.edu))
- **Mukhil Venkataramanan** ([mvenkatarama@binghamton.edu](mailto:mvenkatarama@binghamton.edu))  
- **Gurusaran Venkatachalam Rajarajacholan** ([gvenkatachal@binghamton.edu](mailto:gvenkatachal@binghamton.edu))  
