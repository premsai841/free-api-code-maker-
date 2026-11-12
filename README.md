# APIFREE AI Hub

**APIFREE AI Hub** is a free-first, hybrid AI workspace. It combines experimental browser-local inference, optional connections to cloud AI providers, automatic provider fallback, an API playground, and conversation report export in one responsive website.

The project is designed to make AI more accessible without pretending that paid cloud inference can be made unlimited or that providers' quotas can be bypassed.

## What works in this version

- **Unified chat UI** with conversation history retained in the current browser tab.
- **Cloud provider adapters** for Google Gemini, Groq, and OpenRouter using keys supplied by the user.
- **Automatic fallback** across connected providers for rate-limit, quota, temporary server, overload, and network failures.
- **Browser-local model (experimental)** powered by Transformers.js and a small Qwen model. The model downloads to the visitor's device and uses browser-supported compute; first load can be large and may require WebGPU or substantial RAM.
- **Ollama local adapter** for users who run Ollama on their own computer.
- **API Playground** for sending browser-based HTTP requests and generating cURL.
- **Report export** to Markdown, created locally in the browser.
- Responsive premium dark interface. No user account or application backend is required for this static prototype.

## Important: what "free" means

APIFREE AI Hub does **not** generate API keys, provide shared cloud credits, bypass quotas, or unlock paid models for free.

There are three access paths:

1. **Browser-local model:** no API key or cloud inference bill, but it uses the visitor's device and downloads model files. Speed and quality depend on the device and browser.
2. **Ollama local:** no per-request API bill, but the visitor must install Ollama, download a model, and have suitable hardware.
3. **Cloud free tiers:** users connect their own provider keys. Providers set their own free quotas, model access, billing rules, data policies, and terms. Free tiers can change or disappear. For example, OpenRouter currently documents 20 requests per minute and 50 requests per day for its free variants; Groq rate limits are organization-level and depend on the model and account. Check current provider dashboards before relying on a quota.

If all connected providers are unavailable and no local model is ready, chat cannot produce an answer. The app must not claim otherwise.

## Run locally

This first version is a static HTML application; no build step is needed.

1. Clone the repository:

   ```bash
   git clone https://github.com/premsai841/free-api-code-maker-.git
   cd free-api-code-maker-
   ```

2. Serve the folder from a local web server (recommended for module imports and browser security):

   ```bash
   python -m http.server 8000
   ```

3. Open `http://localhost:8000`.

You can also open `index.html` directly, but some browser features and model loading may behave differently under a `file://` URL.

## Deploy with GitHub Pages

1. Open **Settings → Pages** in the repository.
2. Under **Build and deployment**, select **Deploy from a branch**.
3. Select branch `main` and folder `/(root)`.
4. Save and wait for the GitHub Pages workflow to publish.

Expected project URL (deployment must be enabled separately):

`https://premsai841.github.io/free-api-code-maker-/`

The application is static, so the chat UI and browser-local model can be hosted on GitHub Pages. Provider APIs may reject browser requests because of CORS or provider-specific restrictions. A static host cannot keep a shared server-side API key secret.

## Use the AI chat

1. Open **AI Chat**.
2. For cloud access, open **Connections & settings** and add keys obtained from the providers' official dashboards.
3. Alternatively, load **Browser Local AI** or configure Ollama.
4. Choose **Auto · best available** and leave **Auto fallback** enabled.
5. Send a message. The router tries connected providers in the configured order.

### Current cloud adapters

| Provider | Default model / route | API style |
|---|---|---|
| Google Gemini | `gemini-3.8-flash` | Gemini generateContent |
| Groq | `openai/gpt-oss-20b` | OpenAI-compatible chat completions |
| OpenRouter | `openrouter/free` | OpenAI-compatible chat completions |

Model IDs, availability, and free-tier status can change. Update the defaults in `index.html` when providers retire or rename models. The OpenRouter free route selects from models currently available under its free routing rules; it does not guarantee a particular model.

### Fallback behavior

- The app keeps the conversation messages in browser memory for the current tab.
- When a provider returns a recognized quota/rate-limit, temporary server, overload, or network error, Auto mode attempts the next configured and connected engine.
- Invalid credentials and malformed requests are not blindly retried across every provider.
- The next provider receives the available conversation history, but different models can produce different answers and may have different context limits or tool capabilities.
- This version does not guarantee exact answer equivalence, streaming parity, or seamless continuation for every provider.

## Browser Local AI (experimental)

The app loads Transformers.js from jsDelivr and downloads `onnx-community/Qwen2.5-0.5B-Instruct` from the Hugging Face model hub when the user selects **Load local model**.

- First load requires an internet connection and can download hundreds of megabytes.
- Inference then runs in the browser using available local execution backends.
- Performance varies by browser, device memory, CPU/GPU and WebGPU support.
- Closing the tab or clearing browser data may require model files to be downloaded again.
- This is a small model intended to demonstrate local inference, not to match the quality of large hosted models.

## Ollama local setup

1. Install Ollama from [ollama.com](https://ollama.com/).
2. Pull a model, for example:

   ```bash
   ollama pull qwen2.5:3b
   ```

3. Start Ollama and configure it to accept browser-origin requests from the site using its supported `OLLAMA_ORIGINS` setting.
4. Keep Ollama running on the same computer as the browser.
5. In APIFREE AI Hub settings, enable Ollama and enter the local model name.

The default endpoint is `http://localhost:11434`. Browser mixed-content, private-network, and CORS restrictions can prevent the website from reaching a local service, depending on browser and deployment origin. Do not expose your Ollama port publicly without appropriate security controls.

## API Playground

The playground sends requests directly from the browser. It can display response status and body and generate cURL. Many APIs do not allow browser-origin requests; a CORS failure does not necessarily mean the API is down. Use the generated cURL in a terminal or a properly secured backend when browser access is blocked.

## Security and privacy

- Provider keys are stored in the browser's `localStorage` on that device. They are not sent to an APIFREE AI Hub backend because this project has no backend.
- Browser storage is **not a secure vault**. Anyone with access to the browser profile or a script running on the same origin may be able to access saved keys. Avoid shared/public computers and use restricted keys.
- Provider requests send prompts and conversation history to the selected provider. Review that provider's data handling and retention policy before sending sensitive information.
- Browser-local model inference is intended to run on the visitor's device after model download; model files are fetched from the configured model host.
- Never commit API keys to GitHub, put them in source code, or publish a shared key in a static website.
- A public service with centrally managed API keys requires a backend, abuse controls, budget limits, secret management, monitoring, and an ongoing funding source.

## Architecture

This prototype uses a single static `index.html` file:

- UI layer: navigation, chat, provider settings, API playground and report export.
- Provider layer: Gemini, OpenAI-compatible (Groq/OpenRouter), Ollama, and browser-local adapters.
- Router: provider order, availability checks, error classification and fallback.
- State: conversation history in memory; optional provider settings in localStorage.
- Deployment: static hosting such as GitHub Pages.

A production-grade multi-user platform should split these modules into a maintainable frontend and add a secure backend only if centrally managed provider credentials, authentication, shared quotas, or server-side storage are required.

## Known limitations

- This is an initial prototype, not a production-scale hosted AI service.
- No shared cloud API keys, central credits, user accounts, server database, or unlimited inference are included.
- Browser-local inference is experimental and device-dependent.
- Cloud providers may block direct browser calls or change models, quotas, pricing, and API behavior.
- Fallback only handles recognized failures; provider-specific error formats may need updates.
- Conversation history is held in the current tab and is not synchronized across devices.
- Reports export to Markdown; PDF generation and persistent report library are not included yet.
- API Playground requests may be blocked by CORS.
- Do not use this prototype as a secure store for valuable API credentials.

## Contributing

Issues and pull requests are welcome. Please include the browser, provider, model ID, error response (with secrets removed), and steps to reproduce. Never post API keys or private prompts in an issue.

## License

No license file is currently included. Until a license is added, the repository is not explicitly released under an open-source license. Ask the repository owner before reuse or redistribution.
