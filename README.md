# AI Customer Support & FAQ Assistant

An AI-powered customer support automation workflow built with n8n for OBX AI Solutions.

The system receives customer questions through a webhook, uses an AI Customer Support Agent to generate answers grounded in a company FAQ knowledge base, and escalates questions requiring human assistance through an escalation tool.

## Project Overview

Customer support teams frequently handle repetitive questions about services, pricing, integrations, project timelines, and support.

This project demonstrates how an AI agent can automate the first level of customer support while maintaining controlled knowledge boundaries and providing a human escalation path when the AI cannot reliably answer a question.

### Core workflow

```text
Customer Question
       ↓
Webhook
       ↓
AI Customer Support Agent
       ↓
FAQ Knowledge Base
       ↓
 ┌─────┴─────┐
 ↓           ↓
Answer    Escalation
             ↓
     escalate_to_human
             ↓
 Human Support Escalation Record
```

## Key Features

* Customer question intake through an HTTP POST webhook
* AI-powered customer support agent
* FAQ-based knowledge grounding
* Controlled company-specific responses
* Protection against invented pricing and unsupported claims
* Human escalation for questions requiring additional assistance
* Tool calling through `escalate_to_human`
* JSON response through the webhook
* Simple n8n-based workflow architecture
* Designed as a lightweight AI automation portfolio project

## Technology Stack

* **n8n** — workflow automation and orchestration
* **n8n AI Agent** — customer-support reasoning and response generation
* **GPT-5 mini via n8n Gateway credits** — language model used by the agent
* **FAQ Knowledge Base** — company-specific support information
* **Webhook** — customer-question interface
* **Escalation Tool** — human-support escalation mechanism

## Workflow Components

### 1. Webhook — Receive Customer Question

The workflow accepts customer questions through an HTTP POST request.

Expected request format:

```json
{
  "question": "What is AI automation?"
}
```

### 2. AI Customer Support Agent

The AI Agent processes the customer's question and generates the final response.

The agent follows strict knowledge rules:

* Use the provided FAQ as the primary source.
* Do not invent company information.
* Do not invent pricing, services, policies, capabilities, guarantees, or results.
* Escalate when reliable information is unavailable.
* Escalate when the customer explicitly requests human assistance.
* Keep responses concise and professional.

### 3. FAQ Knowledge Base

The knowledge base contains information covering:

* AI automation
* AI agents
* Website integrations
* Small-business automation
* Project delivery
* Pricing
* Customer support
* AI voice agents

The knowledge base allows the assistant to answer common customer questions using controlled company information rather than relying on unsupported assumptions.

### 4. Human Escalation

When a customer asks for human assistance or the available knowledge is insufficient, the AI Agent calls:

```text
escalate_to_human
```

The tool receives:

```text
customer_question
```

and creates a human-support escalation record.

The current implementation demonstrates the **escalation mechanism**, but does not claim that a human employee has already been notified.

### Future Escalation Automation

The escalation record can be extended into a complete support workflow by connecting additional tools such as:

* Gmail — send an escalation email to the support team
* Slack — notify a support channel or team member
* Ticketing system — automatically create a support ticket
* CRM — update the customer's support status
* Other business communication or help-desk tools

This provides a path from the current portfolio prototype to a more complete production-oriented human-in-the-loop support system.

## 5. Respond with Answer

After the AI Agent finishes processing the request, the workflow returns a JSON response:

```json
{
  "answer": "AI automation uses artificial intelligence to handle repetitive business tasks automatically..."
}
```

## Knowledge-Control Strategy

A major design goal of this project is preventing unsupported AI-generated claims.

For example, the assistant should **not** invent an exact price when the FAQ only states that pricing depends on project scope.

Instead, the question is escalated to human support.

This demonstrates an important production AI concept:

```text
Known Information
       ↓
Answer

Unknown / Insufficient Information
       ↓
Human Escalation
```

## Testing

The workflow was tested using PowerShell HTTP POST requests against the n8n webhook.

### Test 1 — FAQ Question

Input:

```json
{
  "question": "What is AI automation?"
}
```

Result:

* Webhook received the request
* AI Agent processed the question
* FAQ information was used
* Relevant answer was returned
* JSON webhook response was successful

**Status: PASS**

### Test 2 — Unsupported Exact Pricing Question

Input:

```json
{
  "question": "How much does your AI voice agent cost?"
}
```

Result:

* AI Agent identified that exact pricing was unavailable
* `escalate_to_human` was triggered
* Customer received an escalation response
* No unsupported price was invented

**Status: PASS**

## Current Limitations

This is a portfolio demonstration rather than a production customer-support system.

Current limitations include:

* The escalation tool creates an escalation record rather than directly notifying a human.
* Gmail/Slack/ticketing integration is not currently connected.
* The FAQ knowledge base is intentionally limited to the demonstration company's information.
* Production deployment would require additional security, monitoring, authentication, logging, evaluation, and error handling.
* A production AI voice implementation would require an appropriate voice/telephony integration.

## Future Improvements

Possible future extensions include:

1. Connect Gmail for automatic escalation emails.
2. Connect Slack for real-time support notifications.
3. Create support tickets automatically.
4. Add CRM integration.
5. Add conversation history.
6. Add more advanced RAG retrieval.
7. Add automated evaluation and monitoring.
8. Add authentication and webhook security.
9. Add a website chat interface.
10. Connect a voice/telephony provider for a production voice-support implementation.

## Portfolio Skills Demonstrated

This project demonstrates practical experience with:

* AI Agents
* Prompt engineering
* Knowledge grounding
* FAQ-based AI support
* Tool calling
* Human-in-the-loop workflows
* Webhooks
* Workflow automation
* JSON-based API communication
* AI response control
* Escalation handling
* n8n AI workflow design
* Production-oriented AI architecture concepts

## Project Architecture

```text
                    ┌──────────────────────┐
                    │      Customer        │
                    │       Question       │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │       Webhook        │
                    │   POST / question    │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ AI Customer Support   │
                    │       Agent           │
                    └──────────┬───────────┘
                               │
                    ┌──────────┴──────────┐
                    │                     │
                    ▼                     ▼
             ┌─────────────┐      ┌─────────────────┐
             │ FAQ / KB     │      │ Human Escalation│
             │ Grounding    │      │     Tool        │
             └─────────────┘      └────────┬────────┘
                                           │
                                           ▼
                                  ┌─────────────────┐
                                  │ Escalation      │
                                  │ Record          │
                                  └─────────────────┘
                                           │
                              Future: Gmail / Slack /
                              Ticketing / CRM
```

## Project Status

**Status: Complete — Portfolio Frozen**

The current workflow has been successfully tested for both the normal FAQ-answering path and the human-escalation path.

The project is intentionally frozen at this stage as a portfolio demonstration. Future integrations can be added separately without changing the demonstrated core workflow.

## Author

**Obulisivananthan VR**

AI Automation | GenAI Applications | Workflow Automation | AI Agents
