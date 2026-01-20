# 🚀 LEAP Stacks 2 — The AI Launchpad for AWS

**Launch. Explore. Adapt. Prototype.**

Deploy production-ready **AI agents, RAG systems, and full-stack chatbots** to your **own AWS account in minutes** — **no AWS expertise required**.

> Stop wrestling with infrastructure. Start shipping agents.

---

## 🌉 Why this exists

Most AI prototypes die in the **“Production Valley of Despair”** — the gap between a local demo and a secure, observable, production-grade AWS agent.

Web developers can usually get an agent *working*, but turning it into something that is **scalable, secure, observable, and cost-controlled** forces them to solve infrastructure, IAM, observability, and deployment all at once. That’s where momentum dies.

**LEAP Stacks 2 bridges this gap** by providing **infrastructure-as-code** templates that install **directly into your AWS account** via a single **CloudFormation** deployment and integrate easily.

The 12 included stacks launch instantly, can be used as standalone full-stack applications, or they can connect with each other like lego blocks to build larger web applications.

It gives you a **real, production-shaped system on day one**, so you can:
- Learn by interacting with a live system (not diagrams)
- Validate behavior, cost, and latency early
- Adapt the *agent*, not fight the infrastructure
- Decide what’s worth taking to production — and what isn’t

> A prototype isn’t a demo.  
> **It’s an adapted system under real conditions.**

---

## 🔥 What's in LEAP Stacks v2?

- ⚡ **Zero-to-Production in minutes**  
  Single-click deployment. No CLI, CDK, or Terraform required.

- 🛡️ **Zero “Bill Shock” by Default**  
  Every stack includes an optional **2-hour auto-cleanup timer**, removing the #1 fear developers have when experimenting on AWS.

- 📊 **Production Observability from Day One**  
  Live **cost-per-message**, **token usage**, **latency**, and request logs — not afterthoughts.

- 🧠 **Multi-Model Freedom**  
  Instantly switch between **20+ foundation models**, including **Claude Sonnet 4.5** and **Amazon Nova Pro**, etc.

- 🧩 **Opinionated, but Escapable**  
  Start with best-practice defaults — then go under the hood when you’re ready.

---

## 🏗️ Prototype Catalog

LEAP Stacks 2 includes **12 pre-built prototypes**, grouped to take you from cloud fundamentals to advanced agentic workflows.

### 1️⃣ Advanced Agentic Workflows

| Prototype | Description |
|---------|------------|
| **Agentic Automation (n8n)** | Fully hosted n8n for visual AI workflow automation |
| **Voice AI Agent** | Real-time speech-to-speech agent powered by Amazon Nova Sonic 2 |
| **Autonomous Agent Runtime** | Self-updating agent environment with memory + code editor |
| **Agent with MCP Tools** | Serverless agent using Model Context Protocol tools |

### 2️⃣ GenAI Foundations

| Prototype | Description |
|---------|------------|
| **Website Chat (Multimodal)** | Agent that can view websites, answer questions, and monitor changes |
| **RAG Knowledge Base (Aurora)** | Full-stack RAG with relational + vector storage |
| **RAG Knowledge Base (OpenSearch)** | Secure vector search with AI agents |
| **GenAI Chatbot** | Minimal serverless chatbot with secure API Key |

### 3️⃣ AWS Foundations

| Prototype | Description |
|---------|------------|
| **Deploy from GitHub** | Auto-deploy apps from GitHub to AWS Amplify |
| **Authentication & User Management** | Secure auth with admin approval workflow |
| **Serverless Photo Gallery** | Upload and browse images using cloud storage |
| **Serverless CRUD App** | Notes app demonstrating serverless data patterns |

---

## 🚀 Quickstart

1. **Deploy the Environment**  
   Upload `leap-installer-setup.yaml` to the **AWS CloudFormation Console** and choose a secure username/password.

2. **Access the Dashboard**  
   Once the stack is `CREATE_COMPLETE` (~50 seconds), open the LoginURL from **Stack Outputs**.

3. **Login**  
   Enter the username/password from step 1 to **Login**.

4. **Launch a Prototype**  
   Pick a prototype (e.g. *Voice AI Agent*) and click **Launch**.

5. **Explore & Adapt**  
   Edit agent memory live, swap models mid-session, and compare behavior, cost, and latency instantly.

---

## ✨ What’s New in Version 2

- 🚢 **Ship Anywhere**  
  Export full-stack prototypes to **GitHub** in one click — ready to fork and extend.

- 🎨 **Vibe Coding Integration**  
  Import stacks into **Kiro** for spec-driven development with custom steering docs.

- 🌐 **External App Hosting**  
  Securely import and host **Lovable** and **Replit** apps in your AWS account.

- 🧠 **Live Memory Editing**  
  Browser-based agent logic editing for **AgentCore** agents in real time.

---

## 🧠 How This Is Different from “Samples”

Most AWS samples:
- Require cloning repos and running CLIs
- Focus on single services or narrow use cases
- Skip cost controls and observability
- Leave you with cleanup debt

LEAP Stacks:
- Launches **full-stack systems**, not snippets
- Installs into **your AWS account**, not a shared sandbox
- Includes **auto-cleanup and live cost signals**
- Teaches by **running**, not reading

Think of it as:
> **A playground + a blueprint + a safety net**

---

## 🤝 Team

Creator/developer: Basil Fateen
Security reviewers: Erik Hanchet, Cobus Bernard
Contributors and testers: Aaron Hunter, Du'An Lightfoot, Veliswa Boya, Julian Wood, Daniel Geske, Ricardo Sueiras, Salih Gueler, Noureldin Ehab, Ahmed Samir, Awedis Keofteian

---

## 🤝 Community

Join the LEAP Stacks discord server for info, updates, discussions and to get help: https://discord.gg/pvjPsdms

We welcome contributors ❤️

## 📜 License

Distributed under the **MIT License**.  
See the `LICENSE` file for details.
