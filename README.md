# Workday Agent Definition API

## 📋 Navigation
- [🏠 All Versions](#)
- [📖 Current: v1 Spec](versions/v1.md) : [📖 Current: v1 JSON](versions/v1.json)

---

## Introduction

The Workday Agent Definition API intends to facilitate the creation of Agent Definitions that can operate seamlessly with Workday tools and resources. Using industry terminology to cross frameworks and languages, this API intends to make it easy for partners and their ecosystems to allow users to interact with Workday data.

The primary objective of this API is to:

* Create Agents with Skills that can safely and securely access Workday data.
* Discover broadly what capabilities customers and partners can do with other Agents and their Skills.
* Have find grained knowledge of what individual users can access.

Comprehensive specification defining the core concepts of Agents, Skills, and Tools within the Agent System of Record Service. This document provides the foundational understanding for building and integrating with ASOR-managed agents.

## 🎯 Core Concepts Overview

It's important to define the core concepts of Agents, Skills, and Tools within the Agent System of Record Service. This document provides the foundational understanding for building and integrating with ASOR-managed agents.

### Hierarchical Relationship
```
🤖 Agent (Ambient or Delegate Entity)
├── 🎯 Skill (Discrete Capability)
│   ├── 🔧 Tool (External Resource)
│   ├── 🔧 Tool (Processing Service)
│   └── 🔧 Tool (Data Source)
├── 🎯 Skill (Another Capability)
│   ├── 🔧 Tool (API Endpoint)
│   └── 🔧 Tool (ML Model)
└── 🎯 Skill (Communication Capability)
    ├── 🔧 Tool (Messaging Service)
    └── 🔧 Tool (Notification API)
```

### Ambient Agents

An **Ambient** agent can be defined as an agent which can be invoked by events or notifications in the system, and acts independently from human interaction. **Ambient** agents are capable of being started on a schedule and running a skill with another event being pushed or notification being sent.

### Delegate Agents

A **Delegate** agent can be defined as an agent which is invoked by a human, either through direct connection and request, or through a conversation and an orchestrator picking an appropriate skill that the user has access to. A **Delegate** agent can hand off to other skills in other agents to complete it's work, and do not need to send push notifications out when complete.

### Agent Lifecycle in ASOR

| Phase | Description | ASOR API Operation |
|-------|-------------|-------------------|
| **Registration** | Agent is defined and registered in the system | `POST /agentDefinition` |
| **Discovery** | Agent capabilities are made discoverable | Agent Card at `GET /agentDefinition` |
| **Activation** / **Configuration** | Agent is configured with Workday security and becomes available for task execution | Status updates via ASOR Hub |
| **Operation** | Agent executes skills and collaborates | Skill invocation and tool usage with visibility in ASOR Hub |
| **Evolution** | Agent capabilities are updated/enhanced | `PUT /agentDefinition/{id}` (WIP) |
| **Deactivation** | Agent is turned off | Deactivated via ASOR Hub |

---

## 🎯 Skill Definition

### What is a Skill?

A **Skill** represents a discrete, well-defined capability or function that an agent can perform. Skills are the building blocks of agent functionality, representing specific domains of expertise or types of tasks.

#### Skill Characteristics

| Characteristic | Description | Example |
|----------------|-------------|---------|
| **🎯 Atomic** | Single, focused purpose | `analyze_financial_statement` vs `do_everything` |
| **📥 Parameterized** | Accepts defined inputs | Document URL, analysis type, compliance framework |
| **📤 Deterministic Output** | Produces predictable results | Structured analysis report with confidence scores |
| **🔍 Discoverable** | Can be found and invoked programmatically | Listed in Agent Card with parameters schema |
| **🧩 Composable** | Can combine with other skills | Document analysis → Anomaly detection → Report generation |
| **🧠 Context-Aware** | Leverages agent's knowledge | Uses previous analysis history for better results |

## 🚀 Implementation Examples

### Creating an Agent with Multiple Skills

```json
{
  "name": "Customer Service Intelligence Agent",
  "description": "Multi-skilled agent for customer service automation and intelligence",
  "url": "https://acme.com/agent/cust",
  "version": "1.0.0",
  "capabilities": {
    "streaming": true
  },
  "defaultInputModes": [{"type": "text/plain"}],
  "defaultOutputModes": [
    {"type": "application/ms-excel"},
    {"type": "text/plain"}
  ],
  "skills": [
    {
      "id": "sentiment_analysis",
      "name": "Customer Sentiment Analysis",
      "description": "Analyzes customer communication sentiment and emotional state",
      "tags": [{"tag": "cognitive"}],
    },
    {
      "id": "ticket_routing",
      "name": "Intelligent Ticket Routing",
      "description": "Routes customer tickets to appropriate departments",
      "tags": [{"tag": "integration"}],
    }
  ],
  "workdayConfig": [
    {
      "skillId": "sentiment_analysis",
      "workdayResources": [
        {
          "url":"GET",
          "operation":"https://api.workday.com/sentiment/v1"
        }
      ]
    },
    {
      "skillId": "ticket_routing",
      "workdayResources": [
        {
          "url":"POST",
          "operation":"https://api.workday.com/hcm/v1"
        },
        {
          "url":"GET",
          "operation":"https://api.workday.com/classify/v2"
        }
      ]
    }
  ]
}
```

---

## 🔄 Integration Architecture

### Agent-Skill-Tool Interaction Flow

```mermaid
agent creation
    A[Partner Creates Agent] --> B[ASOR POST API]
    B --> C[Creates Agent Registration]

    D[Customer Configures Agent] --> E[ASU Created]
    E --> F[API Client & Secret Created]
    F --> G[API Client Stored]

    H[Workday Tool Request] --> G[API Client used for Token]
    H --> I[Agent Gateway Validates]
    I --> J[Workday Tool invoked]
    J --> I[Tool Response]
    I --> H
```

In this example, the Agent is executing in a non-Workday ecosystem but importantly requires a Workday Financial API to complete it's analysis. It will forward the request to Workdays Agent Gateway, which validates the appropriate security groups defined when the Agent was configuered in the customers tenant.

---

## 🔍 Best Practices

### Agent Design Principles

**✅ Single Responsibility:** Each agent should have a clear, focused purpose rather than trying to do everything. Be clear and mindful about what might be labled as an orchestrator vs a skill based agent.

**✅ Skill Modularity:** Design skills to be readily usable by not just the domain context it was built it, but by many domains.

**✅ Tool Abstraction:** Abstract tool interfaces so skills aren't tightly coupled to specific implementations.

### Common Anti-Patterns

**❌ Monolithic Skills:** Avoid creating skills that try to handle too many different types of tasks.

**❌ Tool Coupling:** Don't hardcode specific tool implementations within skill logic.

**❌ Missing Error Handling:** Always implement proper error handling and fallback mechanisms for tool failures. Can Agents try something else?

---
