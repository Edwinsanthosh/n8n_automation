# 🤖 LinkedIn Job Tracker & AI Career Assistant

> An automated n8n workflow that collects LinkedIn job postings, analyzes technical requirements using AI, extracts relevant skills, and generates personalized cover letters.

---

## 📌 Overview

**LinkedIn Job Tracker** is an AI-powered job automation workflow built with **n8n**.

The workflow automatically runs on a scheduled basis, retrieves job postings through an RSS feed, sends the job description to an LLM, extracts the technical skills required for the position, and generates a personalized cover letter based on the role.

The current workflow is configured with a **Schedule Trigger**, an **RSS Read** node, a **Basic LLM Chain**, **Google Gemini Chat Model**, a **Structured Output Parser**, and a **Google Sheets** node.  

---

# 🚀 Workflow

```text
┌─────────────────────┐
│  Schedule Trigger   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│      RSS Read       │
│                     │
│  LinkedIn Jobs      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   Basic LLM Chain   │
│                     │
│ Analyze Job Posting │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│    Google Gemini    │
│     Chat Model      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Structured Output   │
│      Parser         │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│    Google Sheets    │
│                     │
│   Store Job Data    │
└─────────────────────┘
```

---

# ✨ Features

* ⏰ Scheduled job collection
* 🔗 Retrieves job postings through RSS
* 🤖 AI-powered job description analysis
* 🧠 Extracts technical skills
* 🛠️ Identifies programming languages, frameworks, and tools
* 📝 Generates personalized cover letters
* 📋 Produces structured JSON output
* 📊 Designed to store processed jobs in Google Sheets
* 🔄 Automated end-to-end workflow

---

# 🧩 Workflow Components

| # | Node                     | Purpose                                   |
| - | ------------------------ | ----------------------------------------- |
| 1 | Schedule Trigger         | Starts the workflow automatically         |
| 2 | RSS Read                 | Retrieves LinkedIn job postings           |
| 3 | Basic LLM Chain          | Processes the job posting                 |
| 4 | Google Gemini Chat Model | Performs AI analysis                      |
| 5 | Structured Output Parser | Converts AI response into structured JSON |
| 6 | Google Sheets            | Retrieves/stores spreadsheet data         |

---

# 1️⃣ Schedule Trigger

The **Schedule Trigger** is the starting point of the automation.

The current workflow configures the trigger to run at **10:00**. 

```text
Schedule Trigger
       │
       ▼
Start job tracking workflow
```

### Purpose

Instead of manually executing the workflow, n8n can automatically start the job-tracking process according to the configured schedule.

---

# 2️⃣ RSS Read

The **RSS Read** node retrieves job postings from the configured RSS feed.

The current workflow uses a FetchRSS feed as its source. 

```text
LinkedIn Job Feed
       │
       ▼
    RSS Read
       │
       ▼
 Job Posting Data
```

The RSS data can contain information such as:

```text
Title
Link
Publication Date
Content
Content Snippet
```

This information is passed directly to the Basic LLM Chain. 

---

# 3️⃣ Basic LLM Chain

The **Basic LLM Chain** is responsible for processing the job posting.

The workflow passes the RSS `content` field as the main input to the LLM chain. 

The AI is instructed to perform two primary tasks:

```text
Job Description
       │
       ├───────────────┐
       ▼               ▼
Extract Skills    Generate Cover Letter
       │               │
       └───────┬───────┘
               ▼
        Structured Output
```

---

# 🧠 AI Prompt

The current prompt instructs the model to act as a:

> Professional career advisor and technical recruiter.

The AI is asked to analyze the job posting and extract technical skills from the description.

### Technical Skill Extraction

The model identifies relevant:

* Programming languages
* Frameworks
* Tools
* Explicitly mentioned skills
* Implicitly required technical skills

### Cover Letter Generation

The model is instructed to generate a **250–300 word cover letter** addressing:

* The specific company
* The specific role
* Key responsibilities
* Important requirements
* Relevant matching skills
* Enthusiasm for the position

The requested structure is:

```text
Opening
   ↓
Interest in the role

Middle
   ↓
Relevant skills / experience

Closing
   ↓
Enthusiasm + next steps
```

These instructions and the job fields used by the prompt are defined in the workflow itself. 

---

# 4️⃣ Google Gemini Chat Model

The workflow currently uses the **Google Gemini Chat Model** as the language model connected to the Basic LLM Chain. 

```text
Basic LLM Chain
       │
       │ AI Language Model
       ▼
Google Gemini
       │
       ▼
Analyze Job
```

Gemini receives the job information and the instructions from the LLM chain.

---

# 5️⃣ Structured Output Parser

The **Structured Output Parser** ensures that the AI response follows a predictable JSON structure.

The current workflow defines fields for:

```text
Title
Link
Published Date
About Company and job description
skills
cover letter
```

The parser configuration is included in the workflow JSON. 

### Expected Output

```json
{
  "Title": "Software Developer",
  "Link": "https://example.com/job",
  "Published Date": "2026-09-19",
  "About Company and job description": "Company and job description...",
  "skills": [
    "Python",
    "FastAPI",
    "PostgreSQL",
    "Docker",
    "REST APIs"
  ],
  "cover letter": "Dear Hiring Manager,\n\nI am excited to apply..."
}
```

---

# 🛠️ Recommended Output Structure

For easier processing in later n8n nodes, the skills field can be structured as an array:

```json
{
  "skills": [
    "Python",
    "JavaScript",
    "React",
    "Node.js",
    "REST APIs",
    "SQL"
  ],
  "cover_letter": "Dear Hiring Manager..."
}
```

An even more detailed structure can separate explicitly mentioned skills from inferred skills:

```json
{
  "skills": {
    "explicit": [
      "Python",
      "FastAPI",
      "PostgreSQL"
    ],
    "implicit": [
      "REST API Development",
      "Git",
      "Database Management"
    ]
  },
  "cover_letter": "Dear Hiring Manager..."
}
```

This makes the output easier to use in Google Sheets or another database.

---

# 6️⃣ Google Sheets

The final stage of the current workflow connects the processed result to a **Google Sheets** node.

The imported workflow contains a `Get row(s) in sheet` node after the Basic LLM Chain. 

```text
AI Analysis
     │
     ▼
Structured JSON
     │
     ▼
Google Sheets
```

The intended use of the spreadsheet can be to maintain a centralized job tracker containing information such as:

| Job Title         | Company         | Link | Skills            | Cover Letter     | Date       |
| ----------------- | --------------- | ---- | ----------------- | ---------------- | ---------- |
| Backend Developer | Example Company | Link | Python, FastAPI   | Generated letter | 2026-09-19 |
| Software Engineer | Example Company | Link | JavaScript, React | Generated letter | 2026-09-19 |

---

# 🔄 Complete Data Flow

```text
                    ┌───────────────────┐
                    │  Schedule Trigger │
                    │      10:00        │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │      RSS Read     │
                    │                   │
                    │  LinkedIn Jobs    │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │   Basic LLM Chain │
                    │                   │
                    │ Analyze Job       │
                    │ Extract Skills    │
                    │ Generate Letter   │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │   Google Gemini   │
                    │    Chat Model     │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │ Structured Output │
                    │      Parser       │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │   Google Sheets   │
                    └───────────────────┘
```

---

# 📊 Input Data

The LLM prompt uses information from the RSS feed including:

```text
Title
Link
Published Date
Company information
Job description
```

The current prompt maps these values from the n8n workflow using expressions such as:

```text
{{ $('RSS Feed LinkedIn').item.json.title }}

{{ $('RSS Feed LinkedIn').item.json.link }}

{{ $json.pubDate }}

{{ $json.contentSnippet }}
```

The corresponding fields are present in the workflow configuration. 

---

# 📤 Output Data

The workflow is designed to produce structured job-analysis data.

```json
{
  "Title": "Job Title",
  "Link": "Job Posting URL",
  "Published Date": "Publication Date",
  "About Company and job description": "Job Description",
  "skills": [
    "Technical Skill 1",
    "Technical Skill 2",
    "Technical Skill 3"
  ],
  "cover letter": "250-300 word personalized cover letter"
}
```

---

# 📝 Cover Letter Generation

The generated cover letter follows three major sections.

## Opening

The AI introduces the candidate's interest in the specific role.

```text
I am excited to apply for the Software Developer
position at...
```

## Middle

The AI connects relevant skills to the requirements of the job.

```text
My experience with Python, REST APIs and
database development aligns with...
```

## Closing

The letter finishes with enthusiasm and a request for further discussion.

```text
I would welcome the opportunity to discuss how
my skills could contribute to the team...
```

---

# ⚠️ Important Personalization Consideration

The current workflow provides the LLM with **job information**, but the prompt does not currently provide a detailed candidate profile or resume.

Therefore, truly personalized claims about candidate experience should not be invented.

A stronger future implementation would provide:

```text
Job Description
       +
Candidate Resume
       +
Candidate Skills
       ↓
      LLM
       ↓
Personalized Cover Letter
```

For example:

```text
Candidate Skills:
Python
FastAPI
React
Node.js
PostgreSQL
Docker
```

Then the LLM can match:

```text
Job Requirements
        ↕
Candidate Skills
        ↓
Matching Skills
        ↓
Cover Letter
```

---

# 🧠 AI Matching Process

A future version can make the matching process explicit:

```text
                 Job Description
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
       Required Skills      Responsibilities
             │                   │
             └─────────┬─────────┘
                       ▼
                Compare with
               Candidate Profile
                       │
                       ▼
                 Matching Skills
                       │
                       ▼
               Generate Letter
```

This would make the cover letter more relevant to the candidate's actual background.

---

# 🔐 Security

The workflow uses external service credentials.

These should **never be exposed in the GitHub repository**.

The imported workflow contains a configured Google Gemini credential reference. 

When sharing the workflow publicly:

* Remove exposed credential information
* Do not commit API keys
* Do not commit OAuth secrets
* Use n8n credential management
* Replace personal spreadsheet IDs with placeholders

---

# 🚀 Setup

## Prerequisites

You need:

* n8n
* Google Gemini API access
* Google Sheets access
* An RSS feed containing job postings

---

## 1. Import the Workflow

Open n8n and import the workflow JSON.

```text
n8n
 ↓
Import Workflow
 ↓
Linkedin Job Tracker.json
```

---

## 2. Configure Schedule Trigger

Open the Schedule Trigger.

Configure the execution time.

The current workflow is configured for:

```text
10:00
```



---

## 3. Configure RSS Feed

Open the RSS Read node and configure the job feed.

The current workflow uses a FetchRSS URL. 

Make sure the RSS feed returns the required job information.

---

## 4. Configure Gemini

Connect your Google Gemini credential to the Chat Model.

The Gemini node is connected to the Basic LLM Chain as its language model. 

---

## 5. Configure Structured Output Parser

Use a predictable schema such as:

```json
{
  "type": "object",
  "properties": {
    "Title": {
      "type": "string"
    },
    "Link": {
      "type": "string"
    },
    "Published Date": {
      "type": "string"
    },
    "About Company and job description": {
      "type": "string"
    },
    "skills": {
      "type": "array",
      "items": {
        "type": "string"
      }
    },
    "cover letter": {
      "type": "string"
    }
  },
  "required": [
    "Title",
    "Link",
    "Published Date",
    "About Company and job description",
    "skills",
    "cover letter"
  ]
}
```

---

# 📊 Google Sheets Configuration

Configure the Google Sheets node with your spreadsheet and worksheet.

Suggested columns:

```text
Job Title
Company
Job Link
Published Date
Technical Skills
Cover Letter
Status
Applied Date
```

This can turn the workflow into a complete job application tracker.

---

# 🎯 Suggested Job Tracking Structure

A more complete tracker could look like:

| Field          | Description                            |
| -------------- | -------------------------------------- |
| Job Title      | Position name                          |
| Company        | Hiring company                         |
| Job Link       | Original posting                       |
| Published Date | Date job was published                 |
| Skills         | Extracted technical skills             |
| Cover Letter   | AI-generated letter                    |
| Status         | Saved / Applied / Interview / Rejected |
| Applied Date   | Date application was submitted         |

---

# 🔮 Future Improvements

## 🔍 Job Filtering

Add filtering based on:

* Job title
* Location
* Experience level
* Required technologies
* Salary
* Remote/hybrid/on-site
* Company

---

## 🧠 Candidate Matching

Add a candidate profile:

```text
Resume
+
Skills
+
Projects
+
Experience
```

Then compare it against every job.

```text
Job
 ↓
Extract Requirements
 ↓
Compare Candidate Profile
 ↓
Calculate Skill Match
 ↓
Generate Cover Letter
```

---

## 📊 Job Match Score

A future version could calculate a structured match percentage based on predefined criteria.

Example:

```text
Python        ✓
FastAPI       ✓
PostgreSQL    ✓
Docker        ✓
AWS           ✗

Matching Skills: 4 / 5
```

---

## 📩 Notifications

Add notifications through:

* Gmail
* Telegram
* Slack
* Discord

For example:

```text
New matching job found!

Backend Developer
Company: Example Company

Skills:
Python
FastAPI
PostgreSQL

Cover Letter: Generated ✓
```

---

## 🗄️ Database

Instead of relying only on Google Sheets, a future version could use:

```text
PostgreSQL
MySQL
Supabase
```

to store job postings and application history.

---

# 🐛 Troubleshooting

## LLM Output Is Not Valid JSON

Check the Structured Output Parser.

Make sure the model follows the expected schema.

```text
LLM
 ↓
Structured Output Parser
 ↓
Valid JSON
```

---

## Cover Letter Contains Incorrect Experience

The workflow currently does not provide a complete candidate profile to the LLM.

Add candidate information to the prompt:

```text
Candidate Profile:
{{ $json.candidate_profile }}
```

Then instruct the model:

```text
Only mention skills and experience that are
present in the candidate profile.
Do not invent qualifications.
```

---

## RSS Data Is Missing

Check whether the RSS feed contains:

```text
title
link
pubDate
contentSnippet
```

If the feed uses different field names, update the expressions in the LLM prompt.

---

# 📚 What I Learned

This project provides practical experience with:

* ⚡ n8n workflow automation
* 🔗 RSS integration
* 🤖 LLM integration
* 🧠 Prompt engineering
* 🛠️ Technical skill extraction
* 📝 AI-assisted cover letter generation
* 📋 Structured output parsing
* 📊 Google Sheets integration
* 🔄 Multi-step workflow design
* 🐛 Debugging AI workflows

---

# 💡 Key Concept

The main idea behind this project is:

```text
             JOB POSTING
                  │
                  ▼
           Extract Information
                  │
                  ▼
          Identify Requirements
                  │
                  ▼
           Extract Tech Skills
                  │
                  ▼
          Generate Cover Letter
                  │
                  ▼
            Structured Data
                  │
                  ▼
             Job Tracker
```

This transforms unstructured job descriptions into structured, actionable information.

---

# 🏗️ Architecture

```text
┌──────────────────────────────┐
│       SCHEDULE TRIGGER       │
│           10:00 AM           │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│           RSS READ           │
│                              │
│      LinkedIn Job Feed       │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│       BASIC LLM CHAIN        │
│                              │
│  • Analyze Job Description   │
│  • Extract Technical Skills  │
│  • Generate Cover Letter     │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│      GOOGLE GEMINI MODEL     │
│                              │
│       AI Processing          │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│    STRUCTURED OUTPUT         │
│          PARSER              │
│                              │
│   Skills + Cover Letter      │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│        GOOGLE SHEETS         │
│                              │
│       Job Tracker            │
└──────────────────────────────┘
```

---

# 📁 Repository Structure

Recommended GitHub structure:

```text
LinkedIn-Job-Tracker/
│
├── README.md
│
├── workflow/
│   └── linkedin-job-tracker.json
│
├── screenshots/
│   ├── workflow.png
│   ├── rss.png
│   ├── llm.png
│   ├── output-parser.png
│   └── google-sheets.png
│
└── docs/
    └── prompt.md
```

---

# 📸 Workflow Screenshots

Add your n8n workflow screenshot here:

```markdown
![LinkedIn Job Tracker Workflow](screenshots/workflow.png)
```

You can also document individual nodes:

```markdown
## Schedule Trigger

![Schedule Trigger](screenshots/schedule-trigger.png)

## RSS Feed

![RSS Feed](screenshots/rss.png)

## LLM Chain

![LLM Chain](screenshots/llm.png)

## Structured Output Parser

![Output Parser](screenshots/output-parser.png)

## Google Sheets

![Google Sheets](screenshots/google-sheets.png)
```

---

# 🚀 End-to-End Example

```text
10:00 AM
   │
   ▼
Schedule Trigger
   │
   ▼
Fetch LinkedIn Jobs
   │
   ▼
Read Job Posting
   │
   ▼
Analyze with Gemini
   │
   ├───────────────┐
   ▼               ▼
Extract Skills   Generate Cover Letter
   │               │
   └───────┬───────┘
           ▼
    Structured JSON
           │
           ▼
     Google Sheets
           │
           ▼
     📋 Job Tracker
```

---

# 🌟 Final Result

The workflow turns a raw job posting into structured career information:

```text
┌──────────────────────────────┐
│         RAW JOB POST         │
└──────────────┬───────────────┘
               │
               ▼
             🤖 AI
               │
       ┌───────┴────────┐
       ▼                ▼
   🛠️ Skills       📝 Cover Letter
       │                │
       └───────┬────────┘
               ▼
        📊 Job Tracker
```

The result is an automated pipeline that reduces the manual effort involved in reviewing job descriptions and preparing application materials.

---

# 👨‍💻 Author

## Edwin

Built as a learning project to explore:

**AI + Automation + Career Tools + LLM Workflows**

---

<p align="center">

### 🚀 Find jobs. Understand requirements. Prepare smarter.

**Built with n8n + Google Gemini + RSS + Google Sheets**

</p>

---

## 📜 License

This project is intended for learning and experimentation.

When using job-posting data or RSS feeds, make sure your usage complies with the terms of the respective data source and platform.

```
```
