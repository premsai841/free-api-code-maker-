# APIFREE AI Hub — Architecture

## Product goals

- Keep a useful no-key path through local inference.
- Offer optional free-tier cloud providers without promising unlimited access.
- Preserve one conversation while routing requests between providers.
- Keep the first release deployable as a static website.
- Avoid shared secrets in public client-side code.

## Current prototype topology

```text
Browser
  ├── UI: Overview / Chat / Models / API Playground / Reports
  ├── Session conversation state (in-memory)
  ├── Optional provider settings (localStorage)
  ├── Router
  │    ├── Gemini adapter
  │    ├── OpenAI-compatible adapter (Groq)
  │    ├── OpenAI-compatible adapter (OpenRouter)
  │    ├── Ollama local adapter
  │    └── Transformers.js browser-local adapter
  └── Local report export (Markdown)
```

## Request lifecycle

1. The user sends a message.
2. The message is appended to the in-tab conversation.
3. The router builds the ordered list from settings and filters to configured providers.
4. The selected adapter receives the available conversation history.
5. If the adapter returns a recognized quota, rate-limit, overload, temporary server, or network error, Auto mode attempts the next configured adapter.
6. The successful response is appended to the same conversation with its provider label.
7. If all providers fail, the UI displays the collected errors and does not pretend that an answer was generated.

## Provider contracts

Each adapter should accept normalized conversation messages and return plain assistant text or a typed error.

- Gemini uses the Gemini generateContent API and maps assistant messages to the provider's `model` role.
- Groq and OpenRouter use OpenAI-compatible chat completions.
- Ollama uses its local `/api/chat` endpoint.
- Browser Local AI uses Transformers.js and a small ONNX model.

Model IDs and provider endpoints are configuration, not permanent guarantees. Re-check provider docs and availability before changing defaults.

## Production evolution

The static prototype is intentionally backend-free. If a future release needs centrally managed API credits, accounts, synchronized history, or shared provider credentials, introduce:

- A server-side router with provider adapters.
- Secret management outside source control.
- Per-user authentication and abuse prevention.
- Request budgets, concurrency controls and rate limits.
- Usage telemetry that avoids logging raw prompts or secrets by default.
- Provider-specific privacy notices and data-retention controls.
- Health checks and a provider registry with explicit fallback eligibility.

A backend does not create free inference. Any shared cloud usage needs an ongoing source of credits or funding.
