# 🤖 AI-Powered News Automation

<h3 align="center">
  Automated AI & Technology News Digest using n8n + OpenAI
</h3>

<p align="center">
  <b>Collect → Merge → Process → Summarize → Deliver</b>
</p>

---

## 📌 Overview

**AI-Powered News Automation** is an automated news aggregation and summarization workflow built with **n8n**.

The workflow collects news from multiple RSS feeds, combines the collected articles, sends the data to an OpenAI model for analysis, and automatically delivers a concise news digest through Gmail.

The main goal of this project is to explore how **AI, APIs, workflow automation, and external services** can be connected together to build a useful end-to-end automation system.

Instead of manually visiting multiple websites to stay updated, the workflow automatically creates a personalized news digest.

---

# ✨ Features

- 🕐 Automated execution using a Schedule Trigger
- 📰 Collects AI-related news
- 💻 Collects general technology news
- 🔀 Combines multiple RSS feeds
- 📦 Aggregates collected news items
- 🤖 Uses OpenAI for analysis
- 📝 Generates concise article summaries
- 🏷️ Separates AI and Technology news
- 🔗 Includes original article links
- 📧 Automatically sends the digest through Gmail
- ⚡ Fully automated end-to-end workflow

---

# 🧠 How It Works

The workflow follows this pipeline:

```text
                    ┌─────────────────────┐
                    │   Schedule Trigger  │
                    └──────────┬──────────┘
                               │
                 ┌─────────────┴─────────────┐
                 │                           │
                 ▼                           ▼
        ┌─────────────────┐         ┌─────────────────┐
        │  AI Business    │         │    Tech News    │
        │    RSS Feed     │         │     RSS Feed    │
        └────────┬────────┘         └────────┬────────┘
                 │                           │
                 │                           │
                 └─────────────┬─────────────┘
                               ▼
                       ┌──────────────┐
                       │     Merge    │
                       └──────┬───────┘
                              ▼
                       ┌──────────────┐
                       │   Aggregate  │
                       └──────┬───────┘
                              ▼
                       ┌──────────────┐
                       │    OpenAI    │
                       │  Chat Model  │
                       └──────┬───────┘
                              ▼
                       ┌──────────────┐
                       │    Gmail     │
                       └──────────────┘
````

---

# 🔄 Complete Workflow

The workflow contains the following major stages:

| # | Node              | Purpose                           |
| - | ----------------- | --------------------------------- |
| 1 | Schedule Trigger  | Starts the workflow automatically |
| 2 | AI Business       | Collects AI-related news          |
| 3 | Tech News         | Collects technology news          |
| 4 | Merge             | Combines both RSS outputs         |
| 5 | Aggregate         | Groups all news items             |
| 6 | Basic LLM Chain   | Sends the news to the AI model    |
| 7 | OpenAI Chat Model | Analyzes and summarizes the news  |
| 8 | Gmail             | Sends the final digest            |

---

# 1️⃣ Schedule Trigger

## Purpose

The **Schedule Trigger** is the starting point of the workflow.

Instead of manually executing the workflow every time, n8n can start the workflow automatically according to a predefined schedule.

### Example

```text
Schedule
   ↓
Every Day
   ↓
Collect News
   ↓
Process News
   ↓
Send Email
```

The schedule can be configured according to the desired use case.

For example:

```text
Every day
Every few hours
Every week
```

### Why use a Schedule Trigger?

A news automation workflow should not require manual execution.

The Schedule Trigger allows the entire process to run automatically.

---

# 2️⃣ AI Business RSS Feed

## Purpose

The **AI Business** RSS node collects articles related to artificial intelligence and the AI industry.

The RSS feed provides structured information about each article.

Typical fields include:

```json
{
  "title": "Article title",
  "link": "https://example.com/article",
  "pubDate": "2026-09-18",
  "creator": "Author",
  "content": "Article content",
  "contentSnippet": "Short article description"
}
```

### Example topics

The feed can contain news related to:

* Artificial Intelligence
* Generative AI
* AI Agents
* Machine Learning
* AI Companies
* AI Models
* AI Startups
* Enterprise AI
* AI Infrastructure

### Output

In the current workflow, this feed produced approximately:

```text
50 items
```

These items are passed to the Merge node.

---

# 3️⃣ Tech News RSS Feed

## Purpose

The **Tech News** RSS node collects general technology-related news.

The source can contain articles related to:

* Software
* Hardware
* Cloud Computing
* Developer Tools
* Operating Systems
* Browsers
* Technology Companies
* Software Vulnerabilities
* Open Source
* Enterprise Technology

### Output

In the current workflow, this feed produced approximately:

```text
20 items
```

These items are also sent to the Merge node.

---

# 4️⃣ Merge Node

## Purpose

The **Merge** node combines the output from the two RSS feeds.

The workflow has two inputs:

```text
AI Business
     │
     │
     ▼
   Input 1
     │
     ├──────► Merge
     │
     ▼
   Input 2
     ▲
     │
Tech News
```

### Example

```text
AI Business
50 articles
     +
Tech News
20 articles
     =
70 articles
```

The Merge node is configured using the **Append** operation.

This means the items from both inputs are added together into one stream.

### Why use Merge?

Without the Merge node, the two RSS feeds would remain separate.

The Merge node creates a unified dataset that can be processed by the next stages.

---

# 5️⃣ Aggregate Node

## Purpose

The **Aggregate** node collects the individual news items into a single aggregated structure.

Before aggregation:

```text
Item 1
Item 2
Item 3
Item 4
Item 5
...
Item 70
```

After aggregation:

```text
[
    {
        "title": "...",
        "link": "...",
        "pubDate": "...",
        "contentSnippet": "..."
    },
    {
        "title": "...",
        "link": "...",
        "pubDate": "...",
        "contentSnippet": "..."
    }
]
```

This creates a single dataset that can be passed to the LLM.

---

# 6️⃣ Basic LLM Chain

## Purpose

The **Basic LLM Chain** acts as the AI processing stage.

It receives the aggregated news data and passes it to the connected OpenAI Chat Model.

The chain contains instructions describing what the AI should do with the collected articles.

### Main responsibilities

The LLM is instructed to:

1. Analyze the collected news
2. Identify relevant articles
3. Separate AI and technology news
4. Select the desired number of articles
5. Generate concise summaries
6. Preserve the original links
7. Format the final output for email

---

# 7️⃣ OpenAI Chat Model

## Purpose

The OpenAI Chat Model performs the actual language-model processing.

The aggregated RSS data is provided as input to the model.

The model analyzes the available articles and produces the final news digest.

---

# 📝 AI Prompt

A prompt similar to the following can be used:

```text
You are an AI news summarizer handling multiple categories.

Organize the provided news into:

AI NEWS
================

Select the top 3 AI news items.

TECHNOLOGY UPDATES
==================

Select the top 3 technology news items.

For each selected article:

- Keep the original headline
- Write a concise 2-3 sentence summary
- Include the original article link

Keep the output professional, clean, and easy to scan.

Do not invent information.
Use only the information provided in the articles.
```

---

# 🤖 AI Processing Flow

The AI receives:

```text
70 News Articles
       │
       ▼
     OpenAI
       │
       ▼
   Analyze Articles
       │
       ├───────────────┐
       ▼               ▼
   AI NEWS       TECHNOLOGY NEWS
       │               │
       ▼               ▼
   Select 3         Select 3
       │               │
       └───────┬───────┘
               ▼
           Summaries
               │
               ▼
        Formatted Digest
```

---

# 📧 Gmail Output

The Gmail node is responsible for delivering the final AI-generated digest.

The generated content is passed from the Basic LLM Chain into Gmail.

The email contains two major sections:

```text
AI NEWS

Article 1
Summary
Link

Article 2
Summary
Link

Article 3
Summary
Link


TECHNOLOGY UPDATES

Article 1
Summary
Link

Article 2
Summary
Link

Article 3
Summary
Link
```

---

# 📩 Example Output

The final email generated by the workflow looks similar to:

```text
AI NEWS
=======

PAPERCUT ATTACKER USES HUNDREDS OF AI AGENTS
TO COMPROMISE 440+ INSTANCES

A Russian-speaking threat actor used an
autonomous AI-powered multi-agent framework
to develop and execute exploits against
vulnerable systems.

https://example.com/article


ANTHROPIC DISCLOSES AI HACKING INCIDENT

Anthropic reported a security incident
involving an early version of an AI model.

https://example.com/article


CHATGPT FLAW LET A PLANTED PROMPT SEND
A VICTIM'S GMAIL DATA TO ANOTHER ACCOUNT

Researchers demonstrated a prompt-based
data exfiltration scenario.

https://example.com/article


TECHNOLOGY UPDATES
==================

MICROSOFT PATCHES RECORD 974 FLAWS

Microsoft released a major security update
addressing hundreds of vulnerabilities.

https://example.com/article
```

---

# 🧩 Data Flow

The complete data flow can be represented as:

```text
RSS Feed
   │
   ▼
Raw News Items
   │
   ▼
Merge
   │
   ▼
Combined News
   │
   ▼
Aggregate
   │
   ▼
Structured Dataset
   │
   ▼
Basic LLM Chain
   │
   ▼
OpenAI
   │
   ▼
AI Generated Digest
   │
   ▼
Gmail
```

---

# 🧠 Context Window Challenge

During development, the workflow encountered a context-window error when sending a large collection of articles to the LLM.

The error appeared as:

```text
Bad request - please check your parameters

Your input exceeds the context window of this model.
Please adjust your input and try again.
```

This happened because the RSS items contained large amounts of article content.

For example:

```text
70 Articles
     │
     ▼
Full Article Content
     │
     ▼
Large JSON Dataset
     │
     ▼
Large LLM Request
     │
     ▼
Context Window Error
```

The important lesson is:

> **The number of items is not the only factor that determines the size of an LLM request. The amount of text contained in each item also matters.**

---

# 💡 Context Optimization

A better approach is to reduce the amount of unnecessary information before sending the data to the LLM.

Instead of sending:

```text
Title
Full Article Content
Full Description
Metadata
Author
Date
Link
Other RSS Fields
```

the workflow can keep only:

```text
Title
Short Summary
Date
Link
Category
```

### Optimized pipeline

```text
RSS Feed
    ↓
Extract Required Fields
    ↓
Remove Unnecessary Fields
    ↓
Limit Text Length
    ↓
Batch Large Dataset
    ↓
OpenAI
```

This reduces the amount of information sent to the model.

---

# ⚡ Recommended Production Architecture

For larger datasets, the workflow can be improved using batching.

```text
             RSS Sources
                  │
                  ▼
               Merge
                  │
                  ▼
           Preprocessing
                  │
                  ▼
           Split into Batches
                  │
        ┌─────────┼─────────┐
        ▼         ▼         ▼
      Batch 1   Batch 2   Batch 3
        │         │         │
        ▼         ▼         ▼
       AI         AI        AI
        │         │         │
        └─────────┼─────────┘
                  ▼
          Combine Summaries
                  │
                  ▼
             Final AI
                  │
                  ▼
                Gmail
```

This architecture is more suitable when processing a large number of full articles.

---

# 🛠️ Technologies Used

| Technology     | Role                          |
| -------------- | ----------------------------- |
| **n8n**        | Workflow automation           |
| **RSS**        | News data source              |
| **OpenAI API** | AI analysis and summarization |
| **Gmail**      | Automated email delivery      |

---

# 🔐 Credentials

The workflow requires credentials for the services being used.

## OpenAI

An OpenAI API credential is required for the Chat Model.

The credential is configured inside n8n.

Do not expose API keys in the repository.

---

## Gmail

A Gmail credential is required to send the generated digest.

The Gmail node uses the connected account to deliver the email.

---

# 🔒 Security

Never commit API keys, passwords, OAuth credentials, or other secrets to GitHub.

### ❌ Do not store credentials directly in code

```javascript
const OPENAI_API_KEY = "sk-xxxxxxxxxxxxxxxx";
```

### ✅ Use n8n credentials

```text
n8n
  ↓
Credentials
  ↓
OpenAI
  ↓
Gmail
```

Credentials should remain inside the n8n credential system.

---

# 🚀 Installation & Setup

## Prerequisites

Before running the workflow, make sure you have:

* n8n
* OpenAI API access
* Gmail account
* RSS feed URLs

---

# 1. Install / Run n8n

n8n can be run locally or using a hosted n8n instance.

Once n8n is available, open the workflow editor.

---

# 2. Import the Workflow

If the workflow JSON is included in the repository:

```text
n8n
 ↓
Import Workflow
 ↓
Select workflow JSON
```

The workflow will appear in the n8n editor.

---

# 3. Configure AI Business RSS

Open the **AI Business** RSS node.

Configure the desired RSS feed URL.

Example:

```text
RSS Feed URL
     ↓
AI Business News
```

---

# 4. Configure Tech News RSS

Open the **Tech News** RSS node.

Configure the desired technology RSS feed.

---

# 5. Configure Merge

Make sure both RSS nodes are connected to the Merge node.

```text
AI Business ──► Input 1
                 Merge
Tech News ────► Input 2
```

Use the **Append** operation to combine the inputs.

---

# 6. Configure Aggregate

Configure the Aggregate node to collect the required fields from the incoming news items.

Recommended fields:

```text
title
link
pubDate
contentSnippet
category
```

Avoid sending unnecessary large fields whenever possible.

---

# 7. Configure OpenAI

Open the **OpenAI Chat Model** node.

Select your OpenAI credential.

Choose the required model.

Connect the model to the Basic LLM Chain.

```text
Basic LLM Chain
       │
       │ Model
       ▼
OpenAI Chat Model
```

---

# 8. Configure the LLM Prompt

Add the summarization instructions to the Basic LLM Chain.

The prompt should clearly define:

* Number of articles
* Categories
* Summary length
* Output format
* Link requirements
* No-hallucination instruction

---

# 9. Configure Gmail

Open the Gmail node.

Configure:

```text
Recipient
Subject
Email Body
```

Example subject:

```text
Top AI & Technology News
```

---

# 10. Test the Workflow

Use:

```text
Execute Workflow
```

to test the complete pipeline.

Verify:

```text
Schedule Trigger
       ↓
RSS
       ↓
Merge
       ↓
Aggregate
       ↓
OpenAI
       ↓
Gmail
```

---

# 11. Activate the Workflow

Once the workflow works correctly, activate it.

The workflow will then execute automatically according to the configured schedule.

---

# 📁 Project Structure

A recommended repository structure is:

```text
AI-News-Automation/
│
├── README.md
│
├── workflow/
│   └── ai-news-automation.json
│
├── screenshots/
│   ├── workflow.png
│   ├── ai-business.png
│   ├── tech-news.png
│   ├── merge.png
│   ├── aggregate.png
│   ├── openai.png
│   └── gmail.png
│
└── docs/
    └── prompt.md
```

---

# 📸 Workflow Screenshot

<p align="center">

<img src="screenshots/workflow.png" alt="Complete n8n Workflow" width="1000">

</p>

The complete workflow consists of:

```text
Schedule Trigger
       │
       ├──────────► AI Business RSS
       │
       └──────────► Tech News RSS
                         │
                         ▼
                       Merge
                         │
                         ▼
                     Aggregate
                         │
                         ▼
                  Basic LLM Chain
                         │
                         ▼
                    OpenAI Model
                         │
                         ▼
                       Gmail
```

---

# 📊 Workflow Components

## Schedule Trigger

```text
Purpose:
Automatically start the workflow.
```

## AI Business RSS

```text
Purpose:
Collect AI-related news.
```

## Tech News RSS

```text
Purpose:
Collect technology news.
```

## Merge

```text
Purpose:
Combine multiple RSS outputs.
```

## Aggregate

```text
Purpose:
Create a unified news dataset.
```

## Basic LLM Chain

```text
Purpose:
Provide instructions and process
the aggregated news through the LLM.
```

## OpenAI Chat Model

```text
Purpose:
Analyze and summarize the news.
```

## Gmail

```text
Purpose:
Deliver the generated digest.
```

---

# 🎯 Project Objective

The main objective of this project was to understand how an AI-powered automation pipeline can be created using a visual workflow automation platform.

Instead of manually performing each step:

```text
Search News
    ↓
Read Articles
    ↓
Choose Important Articles
    ↓
Write Summaries
    ↓
Create Email
    ↓
Send Email
```

the workflow automates the entire process:

```text
Collect
   ↓
Process
   ↓
Analyze
   ↓
Summarize
   ↓
Deliver
```

---

# 💻 Why n8n?

n8n makes it possible to connect different services and APIs without building every integration from scratch.

In this project, n8n acts as the orchestration layer between:

```text
RSS
 ↓
Data Processing
 ↓
OpenAI
 ↓
Gmail
```

This allows the workflow to be visually designed and easily modified.

---

# 🤖 Why Use an LLM?

Traditional automation can collect and forward articles, but it cannot easily understand the importance and content of each article.

The LLM adds a reasoning and summarization layer.

For example:

```text
70 Articles
     ↓
AI Analysis
     ↓
Important Stories
     ↓
Short Summaries
     ↓
Readable Digest
```

This transforms a large collection of raw articles into a smaller, more useful information digest.

---

# 🧪 Testing

The workflow can be tested manually before enabling the schedule.

### Test checklist

```text
☐ Schedule Trigger executes
☐ AI Business RSS returns articles
☐ Tech News RSS returns articles
☐ Merge combines both sources
☐ Aggregate creates the expected dataset
☐ OpenAI receives the correct input
☐ AI generates the expected format
☐ Gmail receives the generated content
```

---

# 🐛 Troubleshooting

## Context Window Error

### Error

```text
Your input exceeds the context window of this model.
```

### Cause

Too much data is being sent to the LLM.

### Solution

Reduce the input size.

```text
Remove unnecessary fields
        ↓
Shorten article content
        ↓
Use summaries/snippets
        ↓
Batch large datasets
```

---

## OpenAI Authentication Error

### Possible causes

* Invalid API credential
* Incorrect n8n credential
* Expired credential
* Incorrect configuration

### Solution

Check:

```text
n8n
 ↓
Credentials
 ↓
OpenAI
```

and verify the configured credential.

---

## Gmail Sending Error

### Possible causes

* Gmail credential not connected
* Incorrect recipient
* OAuth authorization issue
* Incorrect email configuration

### Solution

Reconnect or verify the Gmail credential in n8n.

---

## RSS Feed Not Returning Data

### Possible causes

* Invalid RSS URL
* Feed unavailable
* Feed format changed
* Network issue

### Solution

Test the RSS feed separately and verify that it returns valid RSS/XML data.

---

# ⚠️ Current Limitations

The current implementation has some limitations.

### 1. Large article content

RSS feeds may return large article bodies.

Sending all of this content directly to an LLM can increase token usage and may exceed context limits.

---

### 2. Duplicate articles

Different feeds may sometimes contain the same story.

A future version can add duplicate detection.

---

### 3. AI selection is prompt-based

The model determines which articles are important based on the provided prompt and article information.

A dedicated relevance-scoring stage could make the selection process more structured.

---

### 4. RSS dependency

The workflow depends on the availability and structure of the configured RSS feeds.

If a feed changes or becomes unavailable, the corresponding workflow branch may stop returning data.

---

# 🔮 Future Improvements

The project can be expanded significantly.

## Data Processing

* [ ] Remove duplicate articles
* [ ] Filter articles by publication date
* [ ] Limit article content length
* [ ] Extract only required RSS fields
* [ ] Categorize articles automatically

## AI

* [ ] Add article relevance scoring
* [ ] Add sentiment analysis
* [ ] Detect trending topics
* [ ] Generate better headlines
* [ ] Generate a daily AI briefing
* [ ] Compare multiple sources covering the same story

## Automation

* [ ] Add Telegram notifications
* [ ] Add WhatsApp notifications
* [ ] Add Discord notifications
* [ ] Add Slack notifications
* [ ] Add mobile notifications

## Storage

* [ ] Store articles in PostgreSQL
* [ ] Store previously processed articles
* [ ] Track article history
* [ ] Prevent duplicate processing

## Dashboard

* [ ] Create a web dashboard
* [ ] Display trending topics
* [ ] Display news categories
* [ ] Display processing statistics
* [ ] Search previous news digests

---

# 📚 Key Concepts Learned

This project provided practical experience with several concepts.

### Workflow Automation

Connecting multiple services into an automated pipeline.

### RSS Feeds

Consuming structured news data from external sources.

### API Integration

Connecting external services to an automation workflow.

### LLM Integration

Using an OpenAI model as part of an automated process.

### Prompt Engineering

Designing instructions that produce predictable and useful output.

### Data Aggregation

Combining multiple streams of data into a unified dataset.

### Context Windows

Understanding how the amount of input data affects LLM requests.

### Data Preprocessing

Reducing unnecessary information before sending data to an LLM.

### Email Automation

Automatically delivering generated content through Gmail.

---

# 🧠 Key Learning

One of the biggest lessons from this project was that building an AI workflow is not simply about connecting an LLM to a data source.

The data needs to be prepared before it reaches the model.

A good AI workflow can be represented as:

```text
        RAW DATA
           ↓
      PREPROCESSING
           ↓
        FILTERING
           ↓
       STRUCTURING
           ↓
           AI
           ↓
        VALIDATION
           ↓
         OUTPUT
```

This is especially important when working with large datasets.

---

# 📈 Project Evolution

### Version 1

```text
RSS
 ↓
Merge
 ↓
Aggregate
 ↓
OpenAI
 ↓
Gmail
```

### Improved Version

```text
RSS
 ↓
Merge
 ↓
Filter
 ↓
Remove Duplicates
 ↓
Extract Required Fields
 ↓
Batch
 ↓
OpenAI
 ↓
Combine Summaries
 ↓
Gmail
```

The second architecture is more suitable for scaling the workflow.

---

# 🌟 Highlights

### 📰 Automated News Collection

Multiple RSS feeds provide the raw news data.

### 🔀 Multi-Source Processing

AI and technology news are combined into a single workflow.

### 🤖 AI-Powered Summarization

OpenAI converts raw news data into concise summaries.

### 📧 Automated Delivery

The final digest is delivered automatically through Gmail.

### ⚡ End-to-End Automation

The entire pipeline can execute without manual intervention.

---

# 🏗️ Architecture Summary

```text
┌───────────────────────┐
│   Schedule Trigger    │
└───────────┬───────────┘
            │
            ▼
 ┌──────────────────────┐
 │     RSS SOURCES      │
 │                      │
 │  AI Business         │
 │  Technology News     │
 └──────────┬───────────┘
            │
            ▼
 ┌──────────────────────┐
 │        MERGE         │
 └──────────┬───────────┘
            │
            ▼
 ┌──────────────────────┐
 │      AGGREGATE       │
 └──────────┬───────────┘
            │
            ▼
 ┌──────────────────────┐
 │    BASIC LLM CHAIN   │
 └──────────┬───────────┘
            │
            ▼
 ┌──────────────────────┐
 │       OPENAI         │
 │                      │
 │ Analyze              │
 │ Select               │
 │ Summarize            │
 │ Format               │
 └──────────┬───────────┘
            │
            ▼
 ┌──────────────────────┐
 │        GMAIL         │
 │                      │
 │   Final News Digest  │
 └──────────────────────┘
```

---

# 🔐 Best Practices

When extending this workflow, consider the following:

### Keep LLM inputs small

Only send the information the model actually needs.

### Validate external data

RSS feeds can contain missing or malformed fields.

### Handle failures

Add error handling for:

* RSS failures
* API failures
* LLM failures
* Gmail failures

### Avoid duplicate processing

Store processed article URLs or IDs.

### Protect credentials

Never expose API keys in the repository.

### Monitor token usage

Large inputs can significantly increase LLM usage.

---

# 📦 Repository

Recommended repository structure:

```text
AI-News-Automation/
│
├── README.md
│
├── workflow/
│   └── ai-news-automation.json
│
├── screenshots/
│   ├── workflow.png
│   └── gmail-output.png
│
└── docs/
    └── prompt.md
```

---

# 🚀 Quick Start

```text
1. Install n8n
        ↓
2. Import workflow
        ↓
3. Configure RSS feeds
        ↓
4. Add OpenAI credentials
        ↓
5. Configure LLM prompt
        ↓
6. Add Gmail credentials
        ↓
7. Test workflow
        ↓
8. Activate workflow
```

---

# 📌 Example Use Case

Imagine starting your day without manually opening multiple technology websites.

Instead:

```text
08:00 AM
   ↓
n8n starts automatically
   ↓
Collect AI news
   ↓
Collect Technology news
   ↓
Merge articles
   ↓
AI analyzes the articles
   ↓
Select relevant stories
   ↓
Generate summaries
   ↓
Send Gmail
   ↓
📧 News Digest in Inbox
```

The user only needs to open the email.

---

# 👨‍💻 Author

## Edwin

A learning project focused on exploring:

```text
AI
+
Automation
+
APIs
+
LLMs
+
Workflow Orchestration
```

---

# 📜 License

This project is intended for learning and experimentation.

If you reuse the workflow, make sure that you comply with the terms and licenses of the RSS feeds, APIs, news sources, and services you connect to it.

---

# ⭐ Support

If you find this project useful or interesting:

* ⭐ Star the repository
* 🍴 Fork the project
* 🛠️ Experiment with the workflow
* 💡 Build your own automation
* 📢 Share your improvements

---

<p align="center">

## 🚀 Automate the boring stuff. Let AI handle the rest.

<b>Built with n8n + OpenAI</b>

</p>

