# SkillForge

## Explainable Career Match Engine

SkillForge is a desktop career-matching application built with Python and Tkinter that analyzes a candidate's skills and projects against job descriptions.

Instead of producing an unexplained recommendation, SkillForge provides a transparent match score, identifies skill gaps, and generates SHA-256 fingerprints for normalized skill sets.

**Goal:** Build a career tool that is useful, explainable, privacy-conscious, and easy to understand.

---

## Features

* Desktop GUI built with Python and Tkinter
* 0–100 career match score
* Explainable matching algorithm
* Automatic skill extraction
* Skill alias normalization
* Skill-gap analysis
* SHA-256 data fingerprints
* JSON result export
* Automated unit tests
* Zero third-party dependencies

---

## Application Overview

The application provides a simple workflow:

```text
Candidate Profile
       |
       v
Text Normalization
       |
       v
Skill Extraction
       |
       +-------------------+
       |                   |
       v                   v
Candidate Skills      Job Skills
       |                   |
       +---------+---------+
                 |
                 v
         Skill Comparison
                 |
                 v
        Explainable Score
                 |
          +------+------+
          |             |
          v             v
      Match Score    Skill Gaps
          |
          v
     SHA-256 Hash
          |
          v
        Results
```

---

## Getting Started

### Requirements

* Python 3.10 or newer
* Tkinter

Tkinter is included with most standard Python installations.

### Windows

Install Python from the official Python installer and make sure Python is added to PATH.

### Linux

If Tkinter is not installed:

```bash
sudo apt install python3-tk
```

### macOS

Most standard Python installations include Tkinter. If your installation does not, install a Python distribution that includes Tk support.

---

## Installation

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/skillforge.git
```

Navigate into the project:

```bash
cd skillforge
```

No external Python packages are required.

---

## Running the Application

Start the graphical interface:

```bash
python app.py
```

The SkillForge desktop application will open.

The interface allows you to enter:

* Candidate name
* Candidate skills
* Candidate projects
* Job title
* Company
* Job description

Then select **Analyze Match** to generate the results.

---

## Running Tests

SkillForge includes automated tests for the matching engine.

Run:

```bash
python -m unittest discover -s tests -v
```

Expected result:

```text
test_aliases ... ok
test_hash_changes ... ok
test_hash_order_independent ... ok
test_skill_gaps ... ok

----------------------------------------------------------------------
Ran 4 tests

OK
```

---

## How SkillForge Works

SkillForge follows a deterministic processing pipeline.

### 1. Candidate Information

The user enters their skills and previous projects.

Example:

```text
Python
SQL
FastAPI
Docker
Git
Pandas
```

Projects can provide additional evidence of technical experience.

---

### 2. Job Description

The user pastes a job description into the application.

Example:

```text
We are looking for a Backend Software Engineer.

Requirements:
- Python
- FastAPI
- SQL
- Docker
- Git
- REST APIs
- Automated testing
```

---

### 3. Skill Extraction

SkillForge scans the text and identifies recognized technical skills.

The system converts different names for the same technology into canonical skill names.

For example:

| Input       | Canonical Skill  |
| ----------- | ---------------- |
| PostgreSQL  | SQL              |
| MySQL       | SQL              |
| ReactJS     | React            |
| RESTful API | REST API         |
| pytest      | Testing          |
| sklearn     | Scikit-learn     |
| ML          | Machine Learning |

---

### 4. Skill Comparison

The candidate's demonstrated skills are compared with the skills detected in the job description.

For example:

```text
Required:

Python
SQL
Docker
Git
AWS

Candidate:

Python
SQL
Docker
Git
```

The system identifies:

```text
Matched:

Python
SQL
Docker
Git

Skill Gap:

AWS
```

---

## Matching Algorithm

SkillForge intentionally uses a deterministic scoring model rather than a black-box AI model.

The basic formula is:

```text
Skill Coverage × 85
+
Project Experience Bonus
=
Match Score
```

### Skill Coverage

Skill coverage is calculated as:

```text
Matched Required Skills
-----------------------
Detected Required Skills
```

For example:

```text
4 matched skills
----------------
5 required skills

= 80% coverage
```

The project experience bonus rewards candidates who demonstrate hands-on work.

The final score is capped at 100.

Because the formula is visible in the source code, the user can understand why a particular score was produced.

---

## Skill-Gap Analysis

SkillForge does more than calculate a match percentage.

It identifies the specific skills that a candidate does not currently demonstrate.

Example:

```text
Match Score: 78/100

Matched Skills:
- Python
- SQL
- Docker
- Git

Skill Gaps:
- AWS
- React
```

This makes the application useful as a career-development tool as well as a job-matching tool.

---

## SHA-256 Hashing

SkillForge generates SHA-256 fingerprints from normalized skill sets.

For example:

```text
python|sql|docker
       |
       v
    SHA-256
       |
       v
7e0c...a91f
```

The same normalized skill set produces the same fingerprint regardless of ordering.

For example:

```text
Python
SQL
Docker
```

and:

```text
Docker
Python
SQL
```

produce the same fingerprint.

### Why use hashing?

The hashing component demonstrates understanding of:

* Cryptographic hashing
* Data integrity
* Deterministic identifiers
* Privacy-aware application design
* Data normalization

### Important Security Distinction

SHA-256 is hashing, not encryption.

A SHA-256 hash should not be described as a method of making sensitive information anonymous.

If the possible input values are predictable, an attacker may be able to guess the original data by hashing possible values.

For a production system, additional security mechanisms would be required.

---

## JSON Export

After analyzing a job, the user can select:

```text
Export JSON
```

The application generates structured output similar to:

```json
[
  {
    "title": "Backend Software Engineer",
    "company": "Northstar Labs",
    "score": 92,
    "matched": [
      "docker",
      "fastapi",
      "git",
      "python",
      "rest api",
      "sql"
    ],
    "missing": [
      "aws"
    ],
    "candidate_fingerprint": "7e0c...",
    "job_fingerprint": "91af..."
  }
]
```

This makes the matching engine easier to integrate into a future API, database, or web application.

---

## Project Structure

```text
skillforge/
|
├── app.py
|
├── tests/
|   └── test_engine.py
|
├── data/
|
├── README.md
|
├── SECURITY.md
|
├── requirements.txt
|
└── .gitignore
```

### app.py

Contains the primary application:

* Graphical interface
* Skill extraction
* Text normalization
* Matching algorithm
* Score calculation
* Skill-gap analysis
* SHA-256 hashing
* JSON export

### tests/

Contains automated tests for the matching engine.

### SECURITY.md

Documents hashing limitations and security considerations.

---

## Technology Stack

| Technology          | Purpose           |
| ------------------- | ----------------- |
| Python              | Core application  |
| Tkinter             | Desktop GUI       |
| Regular Expressions | Skill extraction  |
| Sets                | Skill comparison  |
| SHA-256             | Data fingerprints |
| JSON                | Data export       |
| unittest            | Automated testing |
| Git                 | Version control   |
| GitHub              | Project hosting   |

---

## Design Decisions

### Why Tkinter?

Tkinter allows the application to run with minimal setup because it is included with most Python installations.

This makes the project easy for recruiters or developers to clone and run.

### Why not use AI immediately?

The first version intentionally uses deterministic logic.

This makes the system:

* Predictable
* Explainable
* Testable
* Easy to debug

A future AI layer could provide semantic matching while the deterministic engine remains underneath it as a validation and explainability layer.

---

## Future Development

SkillForge is designed to evolve into a larger career platform.

### Version 2

Potential improvements:

* Resume PDF upload
* Multiple job postings
* Match history
* Career dashboard
* Charts and visualizations
* Improved desktop interface
* Recommended skills to learn

### Version 3

The architecture could evolve into:

```text
React Frontend
       |
       v
FastAPI Backend
       |
       v
PostgreSQL
       |
       +----------------+
       |                |
       v                v
Candidate Profiles   Job Postings
       |
       v
    Match Engine
       |
       v
 Career Analytics
```

### AI and Machine Learning

Future versions could incorporate:

* Resume semantic analysis
* Job-description embeddings
* Semantic skill matching
* Personalized career recommendations
* Recommended learning paths
* LLM-generated explanations

The deterministic matching engine could remain underneath the AI layer to provide validation and explainability.

---

## Security Roadmap

A production version should implement:

* HTTPS/TLS
* Authentication
* Authorization
* Encryption at rest
* Secure secret management
* Input validation
* Rate limiting
* Audit logging
* Data retention policies
* User data deletion
* Privacy controls

Do not upload real resumes containing sensitive personal information to a public GitHub repository.

---

## Example Use Case

A student applying to multiple internships could use SkillForge to quickly compare their profile against different job descriptions.

For example:

```text
Backend Engineer       92%
Data Analyst            86%
Software Engineer      81%
Cloud Engineer          64%
Frontend Engineer       42%
```

The application can then show why each score was produced and which skills are missing.

Instead of simply answering:

> "You are a 92% match."

SkillForge attempts to answer:

> "Why are you a 92% match, and what skills could improve your match?"

