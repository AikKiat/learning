---
layout: post
title: From Virtual Machines to Docker Containers
description: How containerisation changed software deployment
---
### How I used Redis as a write-aside cache for my latest internship project.

My latest internship project involved doing up an AI-driven fullstack system. A key component was a chatbot that users could use to interact with the AI.

I wanted to make this feature-rich as possible, of course within practical means. After all, too many features lead to too many implementations which entails a more demanding, and complicated design that can be hard to optimise later on. 

So, I took inspiration from modern LLM chat websites such as ChatGPT, and Microsoft Copilot.

![Screenshot 2026-01-11 185157.png]({{site.baseurl}}/learning/Screenshot 2026-01-11 185157.png)

- Above, an image of Copilot's chat interface.

Therefore, consolidating all of the user-focused design features and avoiding anything superfluous, I hence summarised the key components the chat system should have:

	- A button to create a new chat
    - A sidebar to expand to show new chat titles
    - A title displaying current chat title
    - A main dialog area showing user's conversations with the AI
    - A text box accepting user input and of course a simple button for submit.
    
So, rudimentarily considering the current context to be a single instance interaction (1 user talking to the AI), the workflow I devised was something like this:

![Screenshot 2026-01-11 191331.png]({{site.baseurl}}/learning/Screenshot 2026-01-11 191331.png)


A possible user-workflow diagram can be like this:
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


Therefore, there are some key stages where chat data need to be saved and retrieved from the data layer:

> [1]: While chat window is opened:

- 1. When user clicks to create a new chat, save the chat data from current conversation. Show new empty chat box to start conversations.

- 2. When a user chooses to view other chats apart from the current one, save the present chat conversation first, before displaying the other chats in the main chat box.

- 3. When user closes the chat window, save the chat data from the latest conversation. Since beforehand in (1) and (2) we have been saving chat data from other conversations not already marked as the present one, we do not need to do a "batch saving" of all chat conversations whenever the user closes the chatbot window.

> [2]: When opening the chat window:

- 1. First, we want to fetch all chat titles from the data layer. These chat titles can then be displayed on the sidebar which can be expanded or hidden. Upon opening the chat, the user is greeted with an empty chatbox depicting a brand new conversation that can be started. Same sequence as ChatGpt and Copilot.

- 2. The user can then start messaging the AI **either** on this new chat, or select from the list of already saved and then retrieved chat titles for a past ongoing conversation, and continue from there. This brings us back to the brief workflows mentioned above in [1].


### Implementing a strategy for saving data:


    
    
    

