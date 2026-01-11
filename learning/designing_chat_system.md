# How I Created and Optimised a Realtime AI Chat Messaging System

> My latest internship project involved building an AI-driven fullstack system. A key component was a chatbot that users could use to interact with the AI.

## Introduction

I wanted to make this feature-rich as possible, of course within practical means. After all, too many features lead to too many implementations which entails a more demanding and complicated design that can be hard to optimise later on.

So, I took inspiration from modern LLM chat websites such as **ChatGPT** and **Microsoft Copilot**.

![Screenshot 2026-01-11 185157.png]({{site.baseurl}}/learning/Screenshot 2026-01-11 185157.png)
*Above: Copilot's chat interface*

## Key Design Components

Consolidating all of the user-focused design features and avoiding anything superfluous, I summarised the key components the chat system should have:

- A button to create a new chat
- A sidebar to expand to show chat titles
- A title displaying current chat title
- A main dialog area showing user's conversations with the AI
- A text box accepting user input and a submit button

## User Workflow

Considering the current context to be a single instance interaction (1 user talking to the AI), the workflow I devised was something like this:

![Screenshot 2026-01-11 191331.png]({{site.baseurl}}/learning/Screenshot 2026-01-11 191331.png)

### User Workflow Diagram
```mermaid
flowchart TD
    A[User Opens App] --> B[User Opens Chat Tab]

    B --> C[Load Latest Chat Conversation]

    C --> D{User Action?}

    D -->|Click Sidebar| E[Expand Chat List Sidebar]
    E --> F[Display All Chats]

    F --> G{Select Existing Chat?}
    G -->|Yes| H[Load Selected Chat Messages]
    H --> I[User Sends Message]
    I --> J[AI Generates Response]
    J --> H

    G -->|No| K{Create New Chat?}
    K -->|Yes| L[Create New Chat]
    L --> M[Initialize Empty Chat]
    M --> I

    D -->|Click New Chat| L

    D -->|Send Message in Current Chat| I
```

---

## Data Persistence Strategy

There are some key stages where chat data need to be saved and retrieved from the data layer:

### Stage 1: While Chat Window is Open

| Scenario | Action |
|----------|--------|
| **1.1** User creates a new chat | Save current conversation data, then show empty chat box |
| **1.2** User views another chat | Save current conversation first, then display selected chat |
| **1.3** User closes chat window | Save latest conversation only (previous chats already saved) |

### Stage 2: When Opening the Chat Window

| Scenario | Action |
|----------|--------|
| **2.1** Initial load | Fetch all chat titles for sidebar display; show empty chatbox for new conversation |
| **2.2** User continues | Either start messaging in new chat, or select from saved chat titles |

---

## Implementing the Data Storage Strategy

From the above workflow, we need to expose several REST API endpoints. Let's define them and build the required components for a simple storage system.

> ⚠️ **Note:** Since I no longer have code access to this project (internship ended), I will use placeholder names and example response JSON bodies. This documentation focuses on the design perspective and does not contain any sensitive information.


### API Endpoints

**Referencing the workflow pointers (1.1, 1.2, 1.3):**

#### `POST /chat/data` — Save Chat Data

This endpoint handles both creating new chats and updating existing ones.

**Design Decision:** I initially considered creating a separate endpoint to create a new chat. However, this proved unnecessary since the save endpoint can create a chat entry and store contents automatically.

#### Data Structure (Backend Singleton)

I created a Singleton class with a hashmap in this format:

```json
{
  "chat_id": {
    "chat_title": "<string>",
    "messages": ["earliest message", "...", "latest message"]
  }
}
```

#### Request Body Schema

```json
{
  "chat_id": "<string>",
  "chat_title": "<string>",
  "messages": ["earliest message", "...", "latest message"]
}
```

> **Note:** The `messages` array contains messages ordered from earliest (index 0) to latest.

---

#### `GET /chats/{id}` — Get Chat Data by ID

Since the hashmap data structure is not persistent and gets wiped on server restart, we need proper **data modelling**.

---

## Database Design

### Entity Relationship Diagram

Throughout this discussion, we identified two key pieces of information:

1. **Chat titles** — metadata for each conversation
2. **Chat messages** — array of messages per conversation

```mermaid
erDiagram
    CHAT_TITLES {
        int id PK "surrogate primary key"
        int chat_id UK "logical chat identifier"
        varchar chat_title "display title"
    }

    MESSAGES {
        int id PK "message id"
        int chat_id FK "references chat_titles.chat_id"
        text content "message content"
        timestamp created_at "creation time"
    }

    CHAT_TITLES ||--o{ MESSAGES : "has"
```



  
    



    
    
    

