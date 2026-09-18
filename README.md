# ⚡ n8n Automation Projects

> A collection of my **n8n automation projects** built while learning workflow automation, AI integrations, APIs, data processing, and real-world automation.

<p align="center">

**🔄 Automate → 🤖 Integrate AI → 🔗 Connect Services → 📊 Process Data**

</p>

---

## 📌 About This Repository

This repository contains my experiments, learning projects, and practical implementations using **n8n**.

The goal is to learn how different services, APIs, AI models, and applications can be connected together to create useful automated workflows.

Instead of keeping each workflow as an isolated experiment, this repository acts as a **central collection of my n8n automation projects**.

---

# 🧩 Projects

| # | Project                                                | Description                                                                                | Technologies                    |
| - | ------------------------------------------------------ | ------------------------------------------------------------------------------------------ | ------------------------------- |
| 1 | 🤖 [AI-Powered News Automation](./AI-News-Automation/) | Collects AI & technology news, summarizes it using an LLM, and delivers a news digest      | n8n, RSS, OpenAI, Gmail         |
| 2 | 💼 [LinkedIn Job Tracker](./LinkedIn-Job-Tracker/)     | Analyzes job postings, extracts technical skills, and generates personalized cover letters | n8n, RSS, Gemini, Google Sheets |
| 3 | 🚧 More Coming Soon                                    | More automation projects will be added as I continue learning                              | —                               |

> The project list will grow as I build and experiment with new workflows.

---

# 🏗️ Repository Structure

```text
n8n_automation/
│
├── README.md
│
├── AI-News-Automation/
│   ├── README.md
│   ├── workflow/
│   │   └── ai-news-automation.json
│   └── screenshots/
│       └── workflow.png
│
├── LinkedIn-Job-Tracker/
│   ├── README.md
│   ├── workflow/
│   │   └── linkedin-job-tracker.json
│   └── screenshots/
│       └── workflow.png
│
└── ...
```

Each project has its own folder and README so that the workflow can be understood independently.

---

# 🔄 What You Can Find Here

The projects in this repository focus on different areas of automation.

### 🤖 AI Automation

Workflows that integrate AI/LLMs into automated processes.

```text
Data
 ↓
AI Model
 ↓
Analysis
 ↓
Structured Output
 ↓
Action
```

### 🔗 API & Service Integration

Connecting different applications and services together.

```text
Service A
   ↓
n8n
   ↓
Service B
   ↓
Service C
```

### 📰 Data Collection

Automating the collection and processing of information from external sources such as RSS feeds and APIs.

### 📊 Data Processing

Using n8n nodes to:

* Filter data
* Transform data
* Merge data
* Aggregate data
* Extract information
* Generate structured output

### 📧 Automated Notifications

Sending processed information through services such as email and other communication platforms.

---

# 🧠 Why I'm Building These Projects

I'm using this repository as a **learning-by-building space**.

Rather than learning n8n only through tutorials, I'm building small real-world workflows to understand:

* How workflows are designed
* How nodes communicate with each other
* How APIs are integrated
* How data flows through a workflow
* How AI models can be integrated
* How to handle structured outputs
* How to debug workflow failures
* How to design scalable automations

---

# 🛠️ Technologies & Services

Some of the technologies and services used across the projects include:

| Technology / Service | Usage                               |
| -------------------- | ----------------------------------- |
| ⚡ **n8n**            | Workflow automation                 |
| 🤖 **OpenAI**        | LLM integration                     |
| ✨ **Google Gemini**  | LLM integration                     |
| 📰 **RSS**           | Data collection                     |
| 📊 **Google Sheets** | Data storage/tracking               |
| 📧 **Gmail**         | Email automation                    |
| 🔗 **REST APIs**     | External service integration        |
| 🐍 **Python**        | Automation / processing experiments |
| 🗄️ **Databases**    | Data storage experiments            |

The technology stack varies from project to project.

---

# 📚 Learning Path

The projects are intended to gradually explore different levels of automation.

```text
                    n8n
                     │
          ┌──────────┼──────────┐
          │          │          │
          ▼          ▼          ▼
        APIs        AI         Data
          │          │          │
          ▼          ▼          ▼
      Integration  LLMs      Processing
          │          │          │
          └──────────┼──────────┘
                     ▼
              Real Workflows
                     │
                     ▼
              Advanced Automation
```

---

# 🔰 Beginner Concepts

Early projects focus on understanding:

* Triggers
* Nodes
* Connections
* Expressions
* Basic data transformation
* HTTP requests
* RSS feeds
* Simple automation

Example:

```text
Trigger
   ↓
Get Data
   ↓
Process Data
   ↓
Send Output
```

---

# 🤖 AI Workflow Concepts

AI-focused projects explore:

* LLM integration
* Prompt engineering
* Structured output
* AI agents
* Context windows
* Token usage
* Data preprocessing
* AI-powered decision making

Example:

```text
Input Data
    ↓
Preprocessing
    ↓
LLM
    ↓
Structured Output
    ↓
Automation
```

---

# 🔌 API Automation

Some workflows may connect multiple external services:

```text
API
 ↓
n8n
 ↓
Transform
 ↓
AI
 ↓
API
 ↓
Notification
```

This helps demonstrate how n8n can act as an orchestration layer between different services.

---

# 🧪 Experiments

Not every workflow in this repository is intended to be a finished production application.

Some projects are experiments created to understand:

* A particular n8n node
* An API
* An AI model
* A workflow pattern
* A data-processing technique
* An automation concept

Experimental workflows may therefore be incomplete or subject to change.

---

# 🐛 Debugging & Lessons Learned

A major part of this repository is documenting problems encountered while building workflows.

Examples include:

### Context Window Problems

Large amounts of data being sent to an LLM can cause context-window errors.

```text
Large Dataset
     ↓
LLM
     ↓
❌ Context Window Error
```

The solution can involve:

```text
Large Dataset
     ↓
Filter
     ↓
Reduce
     ↓
Batch
     ↓
LLM
```

### Data Structure Problems

Automation workflows often require understanding how data moves between nodes.

```text
Node A
 ↓
JSON
 ↓
Node B
 ↓
Transform
 ↓
Node C
```

Understanding the structure of `$json` and expressions is an important part of building reliable n8n workflows.

---

# 📖 Documentation

Each major project contains its own documentation.

A typical project README includes:

```text
Overview
    ↓
Architecture
    ↓
Workflow Steps
    ↓
Node Configuration
    ↓
Input Data
    ↓
AI Prompt
    ↓
Output Structure
    ↓
Setup
    ↓
Troubleshooting
    ↓
Future Improvements
```

---

# 🚀 How to Use These Workflows

Most projects contain an exported n8n workflow JSON file.

### 1. Clone the repository

```bash
git clone https://github.com/Edwinsanthosh/n8n_automation.git
```

### 2. Open n8n

Run your n8n instance.

### 3. Import a workflow

Inside n8n:

```text
Import Workflow
       ↓
Select .json file
       ↓
Configure credentials
       ↓
Test
```

### 4. Configure credentials

Depending on the project, you may need to configure:

```text
OpenAI
Google Gemini
Gmail
Google Sheets
Other APIs
```

### 5. Execute the workflow

Run the workflow manually first to verify that each node works correctly.

### 6. Activate

Once everything works as expected, activate the workflow.

---

# 🔐 Security

**Never commit credentials or API keys to this repository.**

Do not upload:

```text
❌ API Keys
❌ Passwords
❌ OAuth Tokens
❌ Private Credentials
❌ Personal Access Tokens
```

Use n8n's credential management system instead.

Before publishing an exported workflow, check that it does not contain sensitive information.

---

# 📂 Adding a New Project

When adding a new workflow, use this structure:

```text
n8n_automation/
│
├── README.md
│
└── My-New-Project/
    │
    ├── README.md
    │
    ├── workflow/
    │   └── workflow.json
    │
    └── screenshots/
        └── workflow.png
```

Each project should ideally contain:

### `README.md`

Documentation about the project.

### `workflow.json`

The exported n8n workflow.

### `screenshots/`

Screenshots showing the workflow and important outputs.

---

# 📝 Project Documentation Template

For future projects, the README can follow this structure:

```text
# Project Name

## Overview

## Features

## Architecture

## Workflow

## Node-by-Node Explanation

## Input Data

## Processing

## AI Prompt

## Output

## Setup

## Credentials

## Testing

## Troubleshooting

## Lessons Learned

## Future Improvements

## Author
```

This keeps the repository consistent as more projects are added.

---

# 📈 Future Plans

I plan to continue expanding this repository with workflows involving:

* 🤖 AI Agents
* 🧠 LLM-powered automation
* 🔗 REST APIs
* 📊 Data pipelines
* 📧 Email automation
* 💼 Job automation
* 📱 Notification systems
* 🗄️ Database workflows
* 🔍 Web data processing
* 🐳 Docker-based n8n setups
* 🔄 Multi-step AI workflows

---

# 🎯 Goal

The long-term goal of this repository is to build a collection of **practical n8n automation projects** that demonstrate how automation, APIs, AI, and different services can be combined to solve real-world problems.

```text
Learn
  ↓
Build
  ↓
Experiment
  ↓
Debug
  ↓
Document
  ↓
Improve
  ↓
Repeat 🔄
```

---

# 👨‍💻 Author

## Edwin

Learning and building with:

**AI • Automation • APIs • Backend Development • LLM Workflows**

---

<p align="center">

### ⚡ Learn by building. Automate by experimenting.

**n8n Automation Projects**

</p>

---

## ⭐ Projects

Explore the individual projects:

### 🤖 AI-Powered News Automation

Automated AI & technology news collection, summarization, and email delivery.

**Stack:** `n8n` `RSS` `OpenAI` `Gmail`

### 💼 LinkedIn Job Tracker

Automated job-posting analysis with technical skill extraction and AI-generated cover letters.

**Stack:** `n8n` `RSS` `Google Gemini` `Google Sheets`

---

<p align="center">

⭐ **More automation projects coming soon!**

</p>
```

### One correction before you push it

Your GitHub repository name should be exactly reflected in the clone command and links. If your actual repo is named **`n8n_automation`**, keep the structure above. For each individual project, use the actual folder names you create, e.g.:

```text
n8n_automation/
├── README.md
├── linkedin-job-tracker/
│   ├── README.md
│   └── workflow.json
└── ai-news-automation/
    ├── README.md
    └── workflow.json
```

This makes the **root README a portfolio/index page**, while each project gets its own detailed documentation.
