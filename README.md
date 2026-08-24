# AI Customer Support Agent (Knowledge-Base Grounded)

**Category:** AI Agent / Customer Support Automation
**Stack:** n8n, Webhook, AI Agent, Groq Chat Model, Memory, Google Sheets (as Tool)

## The Problem

Support teams get flooded with repetitive questions — order status, policy details, FAQs — and customers wait on replies that could be answered instantly if the answer already exists in a sheet or doc somewhere.

## The Solution

A webhook-triggered AI Agent that answers customer queries using an LLM (Groq) with conversation memory, and can call **two separate Google Sheets as tools** on demand — one for FAQ/policy data, one for product/order data — grounding every response in real data instead of hallucinated answers.

## Workflow Steps

- **Webhook** receives the incoming customer query (e.g. from a chat widget)
- **AI Agent** processes the message using:
  - **Chat Model:** Groq LLM for reasoning and response generation
  - **Memory:** retains conversation context across turns
  - **Tool 1 — Google Sheets:** agent calls this when it needs FAQ/policy information
  - **Tool 2 — Google Sheets:** agent calls this when it needs product/order information
- **Respond to Webhook** — returns the agent's final answer to the caller

## Why This Design

- Giving the agent **tools** instead of stuffing all data into the prompt keeps token usage low and lets the agent fetch only what's relevant to the specific question
- Two separate sheet tools (rather than one merged sheet) keeps FAQ logic and order-data logic independently maintainable
- Memory node means the agent handles multi-turn conversations naturally, not just single Q&A

## Potential Extensions

- Add a fallback branch: if the agent's confidence is low or no tool returns a match, escalate to a human via Slack instead of guessing
- Swap static Google Sheets for a live database/CRM lookup as the tool source for real-time order status
