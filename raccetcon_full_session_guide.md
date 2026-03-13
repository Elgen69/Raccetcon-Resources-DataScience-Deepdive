# Data Science Workflows
## From Analytics Foundations to Applied AI Systems
**Elgen Mar Arinasa** — Data Science Officer, GDG on Campus – USC

---

# What Even Is Data Science?

Most people think data science starts with training a model. It doesn't. It starts with structure — cleaning messy data, defining assumptions, organizing workflows, and documenting reasoning. Those are not optional steps. They are the foundation.

And yes, I made this entire presentation in Markdown because I was too lazy to open PowerPoint. You're welcome.

### The Terms Everyone Mixes Up

| Term | What It Actually Means | Analogy |
|---|---|---|
| **Data Analytics** | Understanding and describing what happened in data | Reading the scoreboard |
| **Data Science** | Structured problem-solving using data | Being the coach who designs the game plan |
| **Machine Learning** | A tool within data science — letting the computer learn patterns | Training a player to recognize opponent moves |
| **AI** | Broader systems that simulate intelligent behavior | The whole team working together |

### Quick Shoutout

My foundation in structured analytics was shaped under **Gerard Ompad**, who taught our Data Analytics course at USC. Gerard is a senior data scientist now based in Denmark working for **Pfizer**. What he drilled into us was that EDA — Exploratory Data Analysis — is not a checkbox you tick before modeling. It's where you build your understanding. Skip it, and your model learns garbage.

### What This Session Covers

- First half: structured analytics and exploratory reasoning — the boring stuff that actually matters
- Hands-on guided coding — you're not just watching, you're doing
- Second half: an actual ML pipeline for malaria detection — the cool stuff that doesn't work without the boring stuff
- Philosophy: **process first, models second**

---

# Deep Dive: Analytics Coursework Showcase

Before any machine learning, this is where it all starts. This is coursework from our Data Analytics class — the foundation before the fancy stuff.

### DATA_ANALYTICS Lectures — Quick Run-Through

| Lecture | Topic | The Point |
|---|---|---|
| **Lecture 1** | Intro to Data Analytics | This is where we learned the difference between analytics and data science. Definitions matter. |
| **Lecture 2** | Visualization Types | Bar Charts, Line Plots, Scatter Plots, Pie Charts, Time Series — before you build a model, you need to *see* your data. |
| **Lecture 4** | Clustering (K-Means, DBSCAN, Hierarchical) | Unsupervised learning — grouping data without labels. This is where analytics starts overlapping with ML. |
| **Lecture 5** | Regression & Feature Engineering | Feature engineering is where domain knowledge meets code. Most underrated skill in data science. |
| **Lectures 6–8** | KNN, SVM, Decision Trees | The classic ML algorithms. We learned them as analytical tools, not as magic boxes. |

### DataAnalytics Activities — The Range

- **Assignment 1**: Balanced Risk Set Matching — healthcare observational study
- **Assignment 2**: Sankey Diagrams, Network Graphs — visual storytelling
- **Assignment 3**: Clustering with the Sessa Empirical Estimator — pharmaceutical data
- **Assignment 4**: Target Trial Emulation — causal inference in Python

Notice the range — healthcare, networks, pharmaceuticals. Data analytics is not limited to one domain. The *method* is what transfers.

### Standalone Projects

- **Flu vs. COVID-19 Trends** (New Jersey, 2020–2024) — An EDA project comparing disease patterns. No ML — just clean analysis, merging datasets, plotting trends.
- **Iris Data Analysis** — The "Hello World" of data science. Even this benefits from structured EDA.

### The Point

All of this — the charts, the clustering, the causal inference, the EDA notebooks — this is the **foundation**. Without this discipline, the ML pipeline I'm about to show you later would fall apart. Structure matters.

---

# DataLab Guided Activities

Alright, now it's your turn. We're doing two hands-on activities on DataCamp DataLab.

I'm not going to just show you the code and have you copy it. I'm going to ask you questions first. Think about what the task is asking before we write anything. That's what real data analysts do — they understand the question, then code.

And honestly I already have the code ready — because I'm lazy enough to prepare but not lazy enough to make a PowerPoint — so we'll walk through it together.

## Activity A: Analyzing Crime in Los Angeles (Python)

- **Platform**: DataCamp DataLab
- **Dataset**: `crimes.csv` — Los Angeles crime data

Three tasks:
1. Which hour has the highest frequency of crimes?
2. Which area has the most night crimes (10 PM – 3:59 AM)?
3. Count crimes by victim age group (0-17, 18-25, 26-34, 35-44, 45-54, 55-64, 65+)

## Activity B: Analyzing Students' Mental Health (PostgreSQL)

- **Platform**: DataCamp DataLab
- **Table**: `students` — international student mental health survey data

The question: For international students, do mental health scores change based on length of stay?

---

# Applied ML Pipeline: Malaria Detection

Everything we've done so far — the EDA, the visualizations, the SQL analysis — that's the *foundation*. Now here's what happens when you build on that foundation with machine learning.

## The Problem

Malaria kills over 600,000 people a year. Diagnosis relies on a trained lab technician looking at blood smear slides under a microscope — counting tiny parasites inside red blood cells by hand. What if they're tired? What if the clinic doesn't have a trained technician?

We asked: **can we automate this with deep learning?** That question became my undergraduate thesis.

## The Pipeline

```
🩸 Blood Smear Image → 🔍 YOLO (find cells) → 🧬 CNN Ensemble (classify) → 🔥 Grad-CAM (explain) → 📊 Diagnosis
```

### Stage 1: YOLO — The Eyes

Imagine someone hands you a photo of hundreds of tiny circles — red blood cells — all packed together. Your job is to draw a box around every single one. That's what YOLO does.

YOLO stands for "You Only Look Once." It scans the entire image in a single pass and draws bounding boxes around every cell. It doesn't know if they're infected — it just finds them. Each cell it finds gets cropped out and sent to the next stage.

Trained on ~1,900 augmented images of full blood smear slides.

### Stage 2: CNN Ensemble — Three Brains, One Vote

Each cropped cell goes through not one, but **three** neural networks:

| Model | Accuracy | What Makes It Different |
|---|---|---|
| **EfficientNet-B0** | 98.5% | Balances depth, width, and resolution efficiently |
| **ResNet50** | 98.1% | Skip connections solve the vanishing gradient problem |
| **DenseNet121** | 96.7% | Every layer talks to every other layer |

Think of it like three radiologists looking at the same X-ray. Each gives a confidence score. We take a weighted average of all three votes — that's **soft voting**.

One model can overfit. One model can memorize noise. Three architecturally diverse models voting together are harder to fool.

And here's the fun part — trained entirely on **CPU**. About 10 hours total. You don't always need expensive hardware.

### Stage 3: Grad-CAM — Show Me Why

This is the part most ML projects skip. And it's the part that matters most in healthcare.

Grad-CAM asks the model: **"Where were you looking when you made that decision?"** It generates a heatmap — red means high attention, blue means it ignored it.

If a model says "this cell is infected" but the heatmap shows it was looking at the background instead of the cell interior — that's a red flag. The prediction might be right, but for the wrong reason.

In healthcare, you can't deploy a "trust me" model. Doctors need to see **why**.

### Stage 4: Diagnosis Output

The final output packages everything:
- Total cells found, infected count, parasitemia rate
- Severity level (Negative, Low, Moderate, High)
- Annotated image — red boxes on infected cells, green on healthy
- Grad-CAM heatmap overlay — visual proof of where the model looked

Raw predictions turned into something a clinician can actually read and trust.

## Live Demo

Let's see it work.

---

# Reflection

## The Arc

```
EDA & Analytics → Structured Data Thinking → Applied ML → Interpretability
     ↑                     ↑                      ↑              ↑
  Deep Dive          DataLab SQL             YOLO + CNN      Grad-CAM
  Coursework          Activities              Pipeline       Heatmaps
```

Everything today is one continuous workflow. The analytics coursework gave us discipline. The DataLab activities taught us to question our data. The ML pipeline showed how those skills scale to real systems. And Grad-CAM showed why accountability in AI matters.

## Questions to Think About

- In your own projects — hackathons, coursework, thesis work — do you spend more time on data or on models?
- If you had to explain your model's decision to a doctor, how would you do it?
- What's one thing from today that changes how you'd approach your next data project?

---

# Closing

The goal today was never to make you experts in YOLO or SQL or any specific tool. It was to show you that real data science is about **structure, reasoning, and accountability**.

The models are just tools. The workflow is the product.

And honestly — if there's one thing I want you to take home, it's this: the "boring" parts of data science — the cleaning, the EDA, the documentation, the questioning — those aren't just steps you rush through to get to the model. **They are the work.** The model is just the last 10%.

Whether you're analyzing crime stats in a Jupyter notebook, querying student mental health data in SQL, or building a malaria detection pipeline — the mindset is the same. Understand your data. Question your assumptions. Document your reasoning. And if your model can't explain itself, don't trust it.

Now if you have questions, I'm here. If you don't, I'll take that as a sign I did my job — or that you're all hungry and want to leave. Either way, I respect it.

Thank you, and enjoy the rest of RACCETCON! 🎤

---

*Built with Markdown because PowerPoint is for people with patience. — Elgen*
