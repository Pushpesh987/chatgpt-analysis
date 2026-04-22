# 📊 ChatGPT App Review Analysis

> A comprehensive sentiment and text analysis of **193,148 real user reviews** from the ChatGPT mobile app — uncovering what users love, what frustrates them, and what drives ratings.

---

## 📁 Project Structure

```
chatgpt-analysis/
├── ChatGPT_Review_Analysis_1.ipynb   # Main analysis notebook
├── chatgpt_reviews.csv               # Raw dataset
├── chatgpt_reviews_enriched.csv      # Cleaned & enriched dataset with sentiment scores
├── rating_distribution.png           # EDA: Rating distribution
├── review_length_dist.png            # EDA: Review length distribution
├── sentiment_overview.png            # Sentiment count + polarity + subjectivity
├── sentiment_vs_rating.png           # Sentiment breakdown per star rating
├── correlation_heatmap.png           # Correlation matrix of key numeric features
├── keywords_pos_neg.png              # Top keywords in positive vs negative reviews
├── top_keywords_all.png              # Top 20 most frequent words across all reviews
├── polarity_subjectivity_scatter.png # Scatter plot: Polarity vs Subjectivity
├── wordclouds.png                    # Word clouds by sentiment category
└── insight_dashboard.png             # Final BI summary dashboard
```

---

## 🎯 Problem Statement

The ChatGPT mobile app has millions of users leaving reviews on app stores. These reviews are a goldmine of product feedback — but reading them individually is impossible at scale.

**Goal:** Use Python-based NLP and data analysis to:

- Understand the overall sentiment landscape of user reviews
- Discover what specific words and themes drive positive vs. negative feedback
- Correlate text-based sentiment with numerical star ratings
- Derive actionable business insights to guide product decisions

---

## 🔧 What I Did — Step by Step

### 1. 📥 Data Loading & Exploration

- Loaded the raw CSV containing ~193K app reviews
- Explored shape, column types, null values, and sample records
- Key columns: `content` (review text), `score` (1–5 star rating), `at` (date), `thumbsUpCount`

### 2. 🧹 Data Cleaning

- **Removed duplicates** and null/empty review texts
- **Dropped irrelevant columns** (e.g., `replyContent`, `repliedAt`)
- **Standardized text**: lowercased, removed punctuation, stripped extra whitespace
- **Filtered out non-English reviews** to maintain analysis consistency
- Created a clean `review_text` column for all downstream NLP

### 3. 📏 Feature Engineering

- **`review_length`**: Word count of each review
- **`polarity`**: TextBlob sentiment polarity score (−1 = very negative, +1 = very positive)
- **`subjectivity`**: TextBlob subjectivity score (0 = objective, 1 = subjective)
- **`sentiment_label`**: Derived categorical label — _Positive_ (polarity > 0.05), _Negative_ (polarity < −0.05), _Neutral_ (in between)

### 4. 📊 Exploratory Data Analysis (EDA)

- Visualized rating distributions, review length histograms
- Analyzed polarity and subjectivity distributions
- Cross-tabulated sentiment labels with star ratings

### 5. 🔍 Text Analysis

- Tokenized reviews after removing stopwords
- Extracted top 20 keywords across all reviews
- Compared most frequent words in positive vs. negative reviews
- Generated word clouds per sentiment category

### 6. 🔗 Correlation Analysis

- Built a correlation matrix across: `ratings`, `polarity`, `subjectivity`, `review_length`
- Visualized with a heatmap to understand feature relationships

### 7. 📈 BI Dashboard

- Synthesized all findings into a single insight dashboard combining:
  - Overall sentiment split (donut chart)
  - Average polarity by star rating
  - Top positive keywords
  - Subjectivity distribution by sentiment

---

## 📉 Visualizations & What They Tell Us

---

### 1. Rating Distribution

![Rating Distribution](rating_distribution.png)

**What it shows:**
A bar chart and pie chart of review counts by star rating (1 to 5).

**Key Insights:**

- **76.5%** of all reviews are **5-star** (147,808 reviews) — an overwhelmingly positive reception
- **11.6%** are 4-star, making the top-2 ratings account for **88.1%** of all feedback
- Only **6.1%** gave 1-star, **1.7%** gave 2-star, and **4.1%** gave 3-star
- The distribution is **heavily right-skewed**, indicating high overall user satisfaction

> **BI Takeaway:** ChatGPT enjoys strong brand loyalty. The few 1-star reviews are disproportionately impactful for perception and must be addressed seriously.

---

### 2. Distribution of Review Lengths

![Review Length Distribution](review_length_dist.png)

**What it shows:**
A histogram of review word counts, with a red dashed line marking the mean.

**Key Insights:**

- The average review is just **8.7 words** — most users leave very short reviews
- The distribution is highly **right-skewed**: the majority of reviews are under 15 words
- Very few reviews exceed 60 words, but those that do tend to be more detailed complaints or praise

> **BI Takeaway:** Short reviews dominate. Keyword-level analysis is more informative than sentence-level analysis for this dataset. Longer reviews may indicate more emotionally engaged users (both highly satisfied and highly frustrated).

---

### 3. Sentiment Analysis Overview

![Sentiment Overview](sentiment_overview.png)

**What it shows:**
Three panels: Sentiment category counts, polarity score distribution, and subjectivity score distribution.

**Key Insights:**

- **143,111 Positive** reviews (74.1%), **43,928 Neutral** (22.7%), **6,109 Negative** (3.2%)
- Polarity is skewed right — most reviews lean positive, but a notable cluster sits near 0 (neutral)
- Mean subjectivity is **0.52**, indicating reviews are slightly more subjective (opinion-based) than objective
- A large spike at subjectivity = 0 represents very short/factual reviews (e.g., "good app", "5 stars")

> **BI Takeaway:** The vast majority of the user base is satisfied. However, the 22.7% neutral segment presents an opportunity — these users can be converted to promoters with targeted improvements.

---

### 4. Sentiment vs Star Rating

![Sentiment vs Rating](sentiment_vs_rating.png)

**What it shows:**
Left: Stacked bar chart of sentiment labels per star rating. Right: Box plot of polarity scores per star rating.

**Key Insights:**

- 5-star reviews are **almost entirely Positive**, while 1-star reviews show a mix — many 1-star reviews still have _neutral_ polarity text
- Polarity scores **rise monotonically** with star rating (1 → 5), validating that TextBlob sentiment aligns with user-given scores
- 1-star reviews have the **widest polarity range**, including some positive polarity outliers — suggesting frustrated users sometimes write sarcastically or ambiguously
- 5-star reviews have median polarity around **0.5**, still showing room for even more positive language

> **BI Takeaway:** The correlation between polarity and rating is confirmed (r = 0.33). NLP-derived sentiment is a reliable proxy for star ratings, enabling automated quality monitoring without explicit ratings.

---

### 5. Correlation Heatmap

![Correlation Heatmap](correlation_heatmap.png)

**What it shows:**
A 4×4 heatmap showing Pearson correlation coefficients between: `ratings`, `polarity`, `subjectivity`, `review_length`.

**Key Insights:**

- **Ratings ↔ Polarity: r = 0.33** — Moderate positive correlation. Higher-rated reviews use more positive language
- **Polarity ↔ Subjectivity: r = 0.55** — Strong positive correlation. More positive reviews tend to be more opinionated/emotional
- **Ratings ↔ Review Length: r = −0.19** — Slight negative correlation. Lower-rated reviews tend to be longer, as users elaborate on complaints
- **Subjectivity ↔ Review Length: r = −0.02** — Virtually no correlation

> **BI Takeaway:** Longer reviews = more likely to be critical. Monitoring long-form negative reviews is a high-signal strategy for catching product pain points early.

---

### 6. Top Keywords: Positive vs Negative Reviews

![Keywords Pos Neg](keywords_pos_neg.png)

**What it shows:**
Side-by-side horizontal bar charts of the top 15 most frequent words in Positive (green) and Negative (red) reviews.

**Key Insights:**

- **Positive keywords**: `app`, `good`, `best`, `nice`, `great`, `helpful`, `amazing`, `love`, `useful` — classic praise vocabulary
- **Negative keywords**: `app`, `bad`, `wrong`, `cant`, `when`, `use`, `answer`, `time`, `dont`, `worst` — frustration with functionality and answers
- `app` and `chatgpt` appear in both — these are contextual terms, not sentiment drivers
- Negative reviews heavily feature action-blocking words like `cant`, `doesnt`, `dont` — pointing to **usability and performance failures**

> **BI Takeaway:** The product team should investigate issues around "wrong answers" (`wrong`, `bad answers`) and access problems (`cant`, `doesnt work`). Positive word reinforcement around `helpful` and `amazing` should be preserved.

---

### 7. Top 20 Most Frequent Words (All Reviews)

![Top Keywords All](top_keywords_all.png)

**What it shows:**
A horizontal bar chart of the 20 most common words across all 193K reviews, with exact frequency counts.

**Key Insights:**

- `app` (49,539) and `good` (38,963) are by far the most dominant terms
- `best` (18,100), `nice` (13,681), `great` (11,772) show that positive descriptors dominate the overall vocabulary
- `helpful` (11,441) and `useful` (7,501) signal that the **core value proposition is resonating**
- `chatgpt` (10,032) and `gpt` (5,089) appear directly, confirming users are referring to the brand itself
- Words like `work`, `help`, `use`, `information` reflect task-oriented use cases

> **BI Takeaway:** Overall vocabulary is positive and utility-focused. The product is perceived as a helpful, high-quality tool. Marketing can leverage this language in user testimonials.

---

### 8. Polarity vs Subjectivity Scatter Plot

![Polarity Subjectivity Scatter](polarity_subjectivity_scatter.png)

**What it shows:**
A scatter plot with each review plotted by polarity (x-axis) vs subjectivity (y-axis), colored by sentiment label.

**Key Insights:**

- **Positive reviews (green)** are clustered in the top-right quadrant — high subjectivity AND high polarity
- **Negative reviews (red)** are spread across the left half — some are very subjective, others more measured
- **Neutral reviews (orange)** cluster near polarity = 0, across all subjectivity levels
- A clear pattern: **the more positive a review, the more opinionated it tends to be** (high subjectivity)
- Very low subjectivity (near 0) reviews appear across all sentiment groups — these are factual, terse reviews

> **BI Takeaway:** Highly satisfied users are also highly expressive. These are the best candidates for testimonials and referral programs. Negative users with high subjectivity are most likely to write elaborate complaints that contain actionable product feedback.

---

### 9. Word Clouds by Sentiment Category

![Word Clouds](wordclouds.png)

**What it shows:**
Three word clouds — one each for Positive, Neutral, and Negative reviews — where word size reflects frequency.

**Key Insights:**

- **Positive cloud**: Dominated by `good`, `best`, `app`, `love`, `nice`, `great`, `amazing` — very emotionally warm vocabulary
- **Neutral cloud**: `app`, `cant`, `chatgpt`, `when`, `use` — functional but ambiguous, possibly mixed experiences
- **Negative cloud**: `wrong`, `bad`, `cant`, `time`, `use`, `dont`, `answer` — frustration around answers, performance, and access
- The neutral cloud uniquely contains `please` — suggesting users are politely requesting improvements

> **BI Takeaway:** The word clouds visually validate the keyword analysis. Negative users cluster around answer quality and access issues. The word "please" in neutral reviews suggests an engaged, patient user base willing to give feedback constructively.

---

### 10. 🎯 Insight Dashboard (Overall BI Summary)

![Insight Dashboard](insight_dashboard.png)

**What it shows:**A 4-panel summary dashboard combining:

- **A) Overall Sentiment Split** – 74.1% Positive, 22.7% Neutral, 3.2% Negative
- **B) Avg Polarity by Star Rating** – Polarity rises from ~0.01 at 1-star to ~0.50 at 5-star
- **C) Top 10 Positive Keywords** – `app`, `good`, `best`, `nice`, `great`, `helpful`, `amazing`, `love`, `chatgpt`, `useful`
- **D) Subjectivity Distribution by Sentiment** – Positive reviews dominate all subjectivity buckets; neutral reviews cluster at 0

**The Complete Story:**
This dashboard is the executive summary of the entire analysis. It confirms that ChatGPT is a **highly regarded product** with a clear majority of satisfied users. The small but vocal minority of negative reviewers flag specific pain points around **answer quality and app functionality**. Sentiment scores (polarity) cleanly track with star ratings, validating the analysis methodology.

---

## 🧠 Overall Business Intelligence (BI) Summary

| Metric                            | Value                       |
| --------------------------------- | --------------------------- |
| Total Reviews Analyzed            | 193,148                     |
| 5-Star Review Share               | 76.5%                       |
| Positive Sentiment (NLP)          | 74.1% (143,111 reviews)     |
| Neutral Sentiment                 | 22.7% (43,928 reviews)      |
| Negative Sentiment                | 3.2% (6,109 reviews)        |
| Avg Review Length                 | 8.7 words                   |
| Polarity–Rating Correlation       | r = 0.33                    |
| Polarity–Subjectivity Correlation | r = 0.55                    |
| Top Positive Keyword              | `good` (38,963 occurrences) |
| Top Negative Keyword              | `bad` (760 occurrences)     |

### 🔑 Key BI Findings

1. **ChatGPT is overwhelmingly well-received**: 3 out of 4 reviews are positive by both star rating and NLP sentiment.
2. **Answer quality is the #1 pain point**: The words `wrong`, `bad`, `doesnt`, `cant` cluster in negative reviews — users are frustrated when ChatGPT gives incorrect or unhelpful answers.
3. **Access & performance complaints come next**: `cant`, `time`, `slow` in negative reviews suggest intermittent availability or response time issues.
4. **Neutral reviews are an opportunity**: 22.7% of users are not yet raving fans — investigating what keeps them neutral could convert them to promoters.
5. **NLP sentiment is a reliable proxy for star ratings**: The correlation (r = 0.33) is statistically significant, enabling automated review monitoring and alerting.
6. **Longer reviews → more critical**: Review length has a slight negative correlation with ratings. A complaint filter based on review length can triage the most detailed feedback.
7. **Positive users are emotionally expressive**: The high polarity + high subjectivity cluster confirms satisfied users are enthusiastic and vocal — ideal for user-generated content and testimonials.

### 💡 Recommendations

| Priority  | Action                                                                                        |
| --------- | --------------------------------------------------------------------------------------------- |
| 🔴 High   | Investigate and fix answer accuracy issues (top negative signal:`wrong`, `bad`)               |
| 🔴 High   | Monitor and reduce app availability issues (`cant`, `login`, `doesnt work`)                   |
| 🟡 Medium | Engage the 22.7% neutral users with in-app surveys to understand friction points              |
| 🟡 Medium | Set up automated sentiment monitoring using TextBlob polarity on new reviews                  |
| 🟢 Low    | Leverage positive reviewers (`love`, `amazing`, `helpful`) for referral/testimonial campaigns |
| 🟢 Low    | Use long-negative-review filters to auto-escalate high-priority support tickets               |
