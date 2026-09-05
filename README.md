# 🔬 AI Research Paper Assistant

### Turn research papers into structured, analysis-ready literature intelligence — automatically.

An AI-powered research automation workflow built with **n8n** that accepts research papers in PDF format, extracts their content, analyzes them using **Google Gemini**, converts the findings into structured research data, prevents duplicate records through DOI-aware matching, and automatically maintains a literature-review database in **Google Sheets**.

---

## 🎯 The Problem

Literature reviews often require researchers to repeatedly:

- Read lengthy research papers
- Identify methods and models
- Extract experimental parameters
- Compare evaluation metrics
- Record quantitative results
- Identify limitations
- Find research gaps
- Organize everything manually in spreadsheets

For large literature reviews, this process becomes repetitive, time-consuming, and difficult to standardize.

**AI Research Paper Assistant automates this pipeline.**

---

## ⚡ What It Does

Upload a research paper and the workflow automatically:

1. Accepts the paper through an n8n form
2. Extracts text from the uploaded PDF
3. Sends the extracted content to Google Gemini
4. Identifies important technical information
5. Converts the AI response into a structured schema
6. Normalizes the extracted data
7. Generates a unique paper identifier
8. Detects duplicate papers using DOI or title + year
9. Appends new papers or updates existing records
10. Stores the final literature intelligence in Google Sheets

<img width="800" height="862" alt="image" src="https://github.com/user-attachments/assets/ee04e0c2-bd1a-4190-9d66-73413c3b47c7" />


<img width="1280" height="612" alt="image" src="https://github.com/user-attachments/assets/28a5ac3e-2c53-4448-85c7-13eaa1f1a5ef" />


---

## 🧠 Workflow Architecture

```text
                  📄 Research Paper
                         │
                         ▼
                  n8n Form Upload
                         │
                         ▼
                  PDF Text Extraction
                         │
                         ▼
                    Google Gemini
                         │
                         ▼
                 Structured Parser
                         │
                         ▼
                 Python Processing
                         │
                         ▼
              Normalize Research Data
                         │
                         ▼
               Generate Unique Key
                  DOI available?
                  /           \
                YES            NO
                 │              │
                DOI       Title + Year
                  \           /
                   ▼         ▼
                  Deduplication
                         │
                         ▼
                    Google Sheets
                    /           \
                   ▼             ▼
             New Paper      Existing Paper
               APPEND           UPDATE
```

---

## 📊 Structured Information Extracted

Each paper is transformed into a standardized research record containing:

| Field | Description |
|---|---|
| `paper_title` | Title of the research paper |
| `year` | Publication year |
| `doi` | Digital Object Identifier |
| `research_domain` | Primary research area |
| `methods_models` | Algorithms, models, or methodologies used |
| `key_parameters` | Important experimental or model parameters |
| `key_metrics` | Evaluation metrics reported |
| `best_results` | Most significant quantitative findings |
| `tools_dataset` | Software tools, datasets, or simulation environments |
| `limitations` | Limitations identified in the study |
| `research_gap` | Potential gap or opportunity for future research |

This creates a consistent dataset that can be used for **literature comparison, research-gap identification, and review-paper preparation**.

<img width="1662" height="732" alt="image" src="https://github.com/user-attachments/assets/baad30bf-58ce-469c-95a2-ece01f084fab" />


---

## 🔍 DOI-Aware Deduplication

One of the key features of the workflow is automatic duplicate handling.

Instead of blindly inserting every uploaded paper as a new record, the workflow creates a normalized **unique key**.

```text
DOI available
      │
      ▼
Normalize DOI
      │
      ▼
doi:<normalized-doi>
```

If a DOI is unavailable:

```text
Paper Title + Publication Year
             │
             ▼
        Normalize Values
             │
             ▼
title:<normalized-title>|year:<year>
```

Google Sheets then uses this key to determine whether the paper should be:

**APPENDED** as a new record

or

**UPDATED** if it has already been processed.

This prevents duplicate literature records and makes the workflow suitable for continuously growing research collections.

---

## 🤖 Why AI Instead of Simple PDF Extraction?

Traditional PDF extraction can retrieve text, but it does not understand which information matters for a literature review.

The Gemini-powered analysis layer identifies and organizes information such as:

- Research methodology
- Models and algorithms
- Experimental parameters
- Performance metrics
- Important results
- Datasets and tools
- Study limitations
- Research gaps

The result is not simply a paper summary.

It is a **structured literature-intelligence record**.

---

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| **n8n** | Workflow orchestration and automation |
| **Google Gemini** | LLM-based research paper analysis |
| **Python** | Data normalization and unique-key generation |
| **Google Sheets** | Structured literature database |
| **JSON / Structured Output** | Standardized AI response format |
| **PDF Extraction** | Research-paper text ingestion |

---

## 📂 Repository Structure

```text
n8n-ai-research-paper-assistant/
│
├── workflow/
│   └── research-paper-assistant.json
│
├── examples/
│   └── sample-output.json
│
├── screenshots/
│   └── workflow-overview.png
│
├── .gitignore
├── LICENSE
└── README.md
```

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Krithika-Sulochana-08/n8n-ai-research-paper-assistant.git
cd n8n-ai-research-paper-assistant
```

### 2. Open n8n

Use your local or hosted n8n instance.

### 3. Import the workflow

Import:

```text
workflow/research-paper-assistant.json
```

into n8n.

### 4. Configure Google Gemini

Create or select your own Gemini credential inside n8n and assign it to the AI model node.

> Credentials are intentionally not included in this repository.

### 5. Configure Google Sheets

Create a Google Sheet containing columns corresponding to the structured output fields.

Connect your own Google Sheets credential and replace:

```text
YOUR_GOOGLE_SHEET_ID
```

with your spreadsheet configuration.

### 6. Activate the workflow

After configuring the required credentials and integrations, activate the workflow.

Upload a research paper through the form and allow the automation to process it.

---

## 📄 Example Output

A processed paper is converted into a structure similar to:

```json
{
  "paper_title": "Example Research Paper",
  "year": 2025,
  "doi": "10.1234/example.2025.001",
  "research_domain": "Wireless Communication and Machine Learning",
  "methods_models": "Machine learning assisted resource allocation",
  "key_parameters": "Transmit power, SNR, bandwidth",
  "key_metrics": "Throughput, spectral efficiency, energy efficiency",
  "best_results": "Improved resource utilization compared with conventional methods.",
  "tools_dataset": "Python-based simulation",
  "limitations": "Evaluation limited to simulated conditions.",
  "research_gap": "Real-world validation under dynamic conditions is required."
}
```

See [`examples/sample-output.json`](examples/sample-output.json) for the complete demonstration record.

---

## 🔐 Security & Privacy

The public workflow template does **not** contain private API keys or authentication secrets.

Users importing the workflow must configure their own:

- Gemini credentials
- Google Sheets credentials
- Google Sheet
- n8n environment

Never commit `.env` files, access tokens, API keys, or exported credentials to a public repository.

---

## 💡 Key Highlights

- 📄 Automated PDF research-paper ingestion
- 🧠 Gemini-powered technical analysis
- 🧩 Structured AI output
- 📊 Standardized literature-review database
- 🔍 DOI-aware duplicate detection
- 🔄 Automatic append/update behavior
- 🐍 Python-based normalization
- 🔬 Automated research-gap extraction
- ⚙️ End-to-end n8n orchestration
- 🔐 Public workflow without embedded secrets

---

## 🌱 Future Enhancements

Potential extensions include semantic search across processed papers, vector-database integration, citation-network analysis, multi-paper comparison, automated literature-review generation, research trend visualization, and retrieval-augmented question answering over the collected papers.

---

## 🎯 Project Goal

The goal of this project is not to replace researchers.

It is to automate the repetitive part of literature analysis so researchers can spend more time on **comparison, interpretation, critical thinking, and discovering meaningful research opportunities**.

---

## 👩‍💻 Author

**Krithika Sulochana Balasundaram**

Built as an exploration of **AI workflow automation, research intelligence, and practical Generative AI applications**.

---

## ⭐ Support

If you find this project useful, consider giving the repository a ⭐.

Suggestions, improvements, and collaboration opportunities are welcome.

---

## 📜 License

This project is available under the **MIT License**.
