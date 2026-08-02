# AskPack

Turn documents into interactive knowledge packs.

AskPack is a full-stack Retrieval Augmented Generation (RAG) platform that transforms static documents into conversational experiences.

Instead of sending someone a PDF, CV, architecture document, meeting notes, project plan, or learning journal, AskPack enables them to ask questions and explore the content through AI.

A CV chatbot is just one use case. The platform is intentionally designed to be generic.

Upload anything.

Ask questions.

Get answers grounded in your own information.

---

# Vision

Traditional documents are passive.

AskPack makes them interactive.

Examples:

- Candidate CVs
- Project documentation
- Architecture Decision Records (ADRs)
- Meeting notes
- Incident reports
- Study notes
- Personal knowledge bases
- Product documentation

The goal is to provide reliable, source-grounded answers rather than generic AI responses.

If information cannot be found in the source material, AskPack should explicitly say so.

---

# Architecture

```text
                  +----------------+
                  |   Next.js UI   |
                  +--------+-------+
                           |
                           v

                  +----------------+
                  |  Spring Boot   |
                  |      API       |
                  +--------+-------+
                           |
        +------------------+------------------+
        |                  |                  |
        v                  v                  v

+---------------+  +---------------+  +---------------+
|      S3       |  |   DynamoDB    |  | Turbopuffer   |
| Documents     |  | Metadata      |  | Retrieval     |
+---------------+  +---------------+  +-------+-------+
                                             |
                                             v

                                    +----------------+
                                    | Amazon Bedrock |
                                    | Claude Sonnet  |
                                    +----------------+
```

---

# Technology Stack

## Frontend

- Next.js
- React
- TypeScript
- Tailwind CSS

## Backend

- Spring Boot
- Java

## Storage

### Amazon S3

Stores source documents:

- PDF
- DOCX
- Markdown
- Text files

### Amazon DynamoDB

Stores metadata:

- Users
- Knowledge Packs
- Documents
- Conversations
- Processing state

## Retrieval

### Turbopuffer

Stores:

- Embeddings
- Chunks
- Indexes
- Retrieval metadata

Provides:

- Semantic search
- Vector search
- Fast retrieval over Knowledge Packs

## AI

### Amazon Bedrock

Provides access to foundation models.

### Claude Sonnet

Used for:

- Question answering
- Summarisation
- Content extraction
- Structured data generation

## Infrastructure

### Terraform

Infrastructure as Code for:

- S3
- DynamoDB
- ECS/Fargate
- IAM
- CloudWatch
- Secrets Manager

## CI/CD

### GitHub Actions

Pipeline:

```text
Build
  ->
Test
  ->
Package
  ->
Terraform Plan
  ->
Terraform Apply
  ->
Deploy
  ->
Smoke Test
```

---

# Core Concepts

## Knowledge Pack

The primary object in the platform.

A Knowledge Pack contains:

```text
Documents
+
Instructions
+
Conversations
+
Retrieval Index
```

Examples:

- Jeffrey's CV
- AI Cafe Notes
- AWS Learning Journal
- CVSP Architecture
- System Design Handbook

---

## Documents

Files uploaded into a Knowledge Pack.

Examples:

```text
pdf
docx
txt
md
```

Documents are stored in S3.

---

## Chunks

Documents are split into smaller pieces.

```text
Document
  ->
Chunk
  ->
Embedding
  ->
Index
```

Chunks are stored and indexed in Turbopuffer.

---

## Retrieval

When a user asks a question:

```text
Question
  ->
Embedding
  ->
Similarity Search
  ->
Relevant Chunks
```

Only relevant context is sent to the LLM.

---

## Grounded Answers

AskPack follows one simple rule:

> If the information is not present in the source material, the answer should say so.

Answers should always be grounded in retrieved content rather than assumptions or hallucinations.

---

# Example Flow

## Upload

User uploads:

```text
Jeffrey-CV.pdf
```

Stored in:

```text
S3
```

---

## Process

```text
PDF
  ->
Text Extraction
  ->
Chunking
  ->
Embedding Generation
  ->
Turbopuffer Indexing
```

---

## Ask

User asks:

```text
What AWS experience does Jeffrey have?
```

---

## Retrieve

```text
Turbopuffer
  ->
Relevant Chunks
```

---

## Generate

```text
Claude Sonnet
  ->
Grounded Answer
```

---

## Respond

```text
Answer
+
Supporting Sources
```

---

# Repository Structure

```text
askpack/

├── frontend/
│   ├── src/
│   ├── public/
│   └── tests/
│
├── backend/
│   ├── src/
│   ├── test/
│   └── pom.xml
│
├── infrastructure/
│   ├── environments/
│   │   ├── dev/
│   │   └── prod/
│   │
│   └── modules/
│       ├── s3/
│       ├── dynamodb/
│       ├── ecs/
│       ├── iam/
│       └── monitoring/
│
├── docs/
│   ├── architecture.md
│   ├── decisions/
│   └── diagrams/
│
├── .github/
│   └── workflows/
│
├── README.md
├── TODO.md
└── PLAN.md
```

---

# Project Goals

This project exists to learn modern cloud-native AI application development.

Goals:

- Build a production-grade full-stack application
- Learn AWS infrastructure
- Learn Terraform
- Learn GitHub Actions
- Learn modern RAG architecture
- Learn Bedrock and foundation models
- Learn DynamoDB data modelling
- Learn AI application observability
- Build something genuinely useful

---

# Non-Goals

Not building:

- A generic chatbot
- A ChatGPT competitor
- An autonomous agent platform
- A workflow orchestration platform

Focus remains on:

```text
Private Knowledge
+
Retrieval
+
Grounded Answers
```

---

# Future Features

## Public Knowledge Packs

Share a pack publicly:

```text
askpack.ai/p/jeffrey-cv
```

and allow others to query it.

## Multiple Personas

Examples:

- Recruiter
- Technical Interviewer
- Architect
- Executive Summary
- Incident Investigator

## Source Citations

Every answer shows:

- Source document
- Relevant excerpt
- Confidence score

## Evaluations

Automated tests for:

- Retrieval quality
- Hallucination detection
- Grounding accuracy

## Agentic Retrieval

Future experimentation with:

- Retriever agent
- Answering agent
- Verification agent
- Citation validator

---

# Guiding Principle

> Documents should not be things we read.
>
> Documents should be things we talk to.
>
> — AskPack
