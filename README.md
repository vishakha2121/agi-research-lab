<div align="center">

# 🧠 AGI Research Lab

### A Modular Artificial Superintelligence Research Platform

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg?style=for-the-badge&logo=python)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.110+-green.svg?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-18+-61DAFB.svg?style=for-the-badge&logo=react)](https://reactjs.org/)
[![Gemini](https://img.shields.io/badge/Gemini-API-orange.svg?style=for-the-badge&logo=google)](https://deepmind.google/technologies/gemini/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

**A practice-grade, CPU-friendly sandbox for building, testing, and evaluating increasingly capable AI systems.**

[Explore the Docs](#-architecture) · [View Features](#-core-features) · [Setup Guide](#-getting-started)

</div>

---

## 📖 Table of Contents

1. [Overview](#-overview)
2. [Core Features](#-core-features)
3. [Tech Stack](#-tech-stack)
4. [Architecture](#-architecture)
5. [Database Schema](#-database-schema)
6. [API Endpoints](#-api-endpoints)
7. [Project Structure](#-project-structure)
8. [Getting Started](#-getting-started)
9. [Usage Guide](#-usage-guide)
10. [Safety & Governance](#-safety--governance)
11. [Future Scope](#-future-scope)
12. [License](#-license)

---

## 🔭 Overview

**AGI Research Lab** is an open, modular research environment designed to explore the fundamental building blocks of Artificial General Intelligence (AGI). 

While traditional AGI research often relies on massive GPU clusters, distributed training frameworks (like DeepSpeed), and heavy enterprise infrastructure (Kubernetes, Kafka), this project takes a **pragmatic, practice-oriented approach**. It leverages the power of modern Large Language Models (Google Gemini) to simulate cognitive architectures, multi-agent collaboration, and self-improvement loops on standard CPU hardware.

This project serves as a comprehensive sandbox for:
- **Researchers** exploring agentic workflows and memory systems.
- **Developers** learning how to orchestrate complex AI pipelines.
- **Interview Candidates** demonstrating full-stack AI engineering skills.

---

## ✨ Core Features

### 1. 🧩 Modular Cognition
The system is not a single monolithic prompt. It is broken down into specialized cognitive modules:
- **Perception Module:** Parses user input and extracts intent.
- **Reasoning Module:** Breaks down complex problems into logical steps.
- **Planning Module:** Creates a step-by-step execution plan.
- **Action Module:** Decides which tools to use and when.
- **Memory Module:** Decides what to remember and what to recall.

### 2. 🧠 Long-Term Memory
Agents don't just forget after a conversation ends.
- **Episodic Memory:** Stores past interactions and specific events.
- **Semantic Memory:** Stores general knowledge and facts learned over time.
- **Vector Search:** Utilizes ChromaDB to perform semantic similarity searches, allowing agents to recall relevant memories based on context, not just keywords.

### 3. 🤝 Multi-Agent Collaboration
Complex tasks are handled by a team of specialized agents working together:
- **Planner Agent:** Receives the task and breaks it into sub-tasks.
- **Executor Agent:** Carries out the sub-tasks using tools and reasoning.
- **Critic Agent:** Reviews the output of the Executor for accuracy, safety, and relevance.
- **Coordinator:** Orchestrates the flow of information between agents.

### 4. 🪞 Reflection & Self-Improvement
The AI doesn't just output the first thing it thinks of.
- **Reflection Loops:** The Critic Agent evaluates the output. If it fails, the Executor tries again with the Critic's feedback.
- **Prompt Versioning:** Every prompt is versioned. If a specific prompt iteration leads to a failure, it is logged, and a feedback loop suggests an improvement.

### 5. 🛠️ Tool Learning & Usage
The platform includes a robust Tool Registry.
- **Dynamic Tools:** Tools can be registered and discovered at runtime.
- **Available Tools:** Calculator, Web Search (Mock), File Reader, Python Executor (Sandboxed), and DateTime.
- **Function Calling:** Uses Gemini's native function calling capabilities to decide when and how to use a tool.

### 6. 📊 Reasoning Benchmarks
We need to measure progress. The platform includes built-in benchmarks:
- **Reasoning QA:** Tests logical deduction and common sense.
- **Memory Tests:** Checks if the agent can recall information provided 10 turns ago.
- **Tool Use Tests:** Evaluates if the agent selects the correct tool for a given problem.

### 7. 🛡️ Safety & AI Governance
An AGI must be safe.
- **Content Filters:** Scans inputs and outputs for harmful content (hate speech, PII, etc.).
- **Policy Engine:** Enforces rules (e.g., "Do not execute Python code that accesses the file system").
- **Audit Logs:** Every action taken by an agent is logged for human review.

### 8. 🌍 World Modeling & Simulation
- **Simulation Environments:** Includes a GridWorld simulation where agents can navigate and solve spatial tasks.
- **World State:** Maintains an internal JSON-based representation of the environment to predict outcomes.

### 9. ⚡ Distributed Execution (Lightweight)
- **Background Workers:** Heavy tasks are offloaded to background threads, preventing the main API from blocking.
- **Task Queue:** Uses a simple SQLite-backed queue for managing experiments.

---

## 💻 Tech Stack

### Backend
| Technology | Purpose |
| :--- | :--- |
| **Python 3.11+** | Core backend language. |
| **FastAPI** | High-performance REST API framework. |
| **SQLAlchemy** | ORM for database interactions. |
| **SQLite** | Lightweight, file-based relational database (no setup required). |
| **ChromaDB** | Local vector database for semantic memory search. |
| **Google Gemini API** | The core LLM engine driving reasoning and generation. |
| **Pydantic** | Data validation and schema management. |
| **Pytest** | Unit and integration testing. |

### Frontend
| Technology | Purpose |
| :--- | :--- |
| **React 18** | Component-based UI library. |
| **Vite** | Blazing fast build tool and dev server. |
| **TailwindCSS** | Utility-first CSS framework for a modern, glassmorphism UI. |
| **Framer Motion** | Smooth animations and page transitions. |
| **Recharts** | Data visualization for benchmarks and usage stats. |
| **React Router DOM** | Client-side routing. |
| **Axios** | HTTP client for API requests. |
| **Zustand** | Lightweight global state management. |

---

## 🏗️ Architecture

The system follows a modular, layered architecture.

```text
       +-------------------------------------------------------+
       |                   React Frontend                      |
       |  (Dashboard, Chat, Agents, Memory, Benchmarks, etc.)  |
       +---------------------------+---------------------------+
                                   | REST API (JSON)
                                   v
       +-------------------------------------------------------+
       |                   FastAPI Backend                     |
       |                                                       |
       |  +----------------+  +----------------+  +---------+  |
       |  |  API Routers   |  |   Middleware   |  | Utils   |  |
       |  +-------+--------+  +-------+--------+  +----+----+  |
       |          |                   |                |       |
       |          v                   v                v       |
       |  +-------------------------------------------------+  |
       |  |                 Service Layer                   |  |
       |  | (Chat, Memory, Tools, Safety, Governance, etc.) |  |
       |  +------------------------+------------------------+  |
       |                           |                           |
       |          +----------------+----------------+          |
       |          |                                 |          |
       |          v                                 v          |
       |  +----------------+               +----------------+  |
       |  |   Agent Core   |               |  Memory Manager|  |
       |  | (Planner, etc) |               |  (ChromaDB)    |  |
       |  +-------+--------+               +----------------+  |
       |          |                                            |
       |          v                                            |
       |  +----------------+                                   |
       |  |  Tool Registry |                                   |
       |  +-------+--------+                                   |
       |          |                                            |
       +----------|--------------------------------------------+
                  |
        +---------+---------+
        |                   |
        v                   v
+-------+-------+   +-------+-------+
|  Gemini API   |   | SQLite DB     |
| (LLM Engine)  |   | (Relational)  |
+---------------+   +---------------+