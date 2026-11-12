# Security notes

## API keys

- Do not commit provider API keys to the repository.
- In the current static prototype, user-entered keys are stored in browser localStorage.
- localStorage is not an encrypted vault. Other scripts executing on the same origin and anyone with access to the browser profile may be able to read it.
- Do not use shared/public devices for provider keys. Use restricted keys and provider-side quotas where available.
- The project does not contain a shared API key or a server-side proxy.

## Prompts and conversation data

- Cloud prompts and chat history are sent to the provider selected for that request.
- Review provider privacy, retention, and training settings before sending confidential information.
- Conversation history currently exists only in browser memory for the active tab.
- Markdown reports are generated locally and downloaded by the browser.

## Local inference

- Browser-local inference downloads model artifacts from the configured model host.
- Ollama requests are sent to the local Ollama service on the user's machine.
- Do not expose Ollama's local API port to the public internet without proper authentication and network controls.

## Public deployment

A static website cannot protect a shared cloud provider key. Never embed a central key in HTML, JavaScript, GitHub Pages configuration, or public repository secrets that are exposed to client builds. A centrally funded public service requires a backend, secret manager, abuse prevention, per-user budgets, monitoring, and an explicit funding source.
