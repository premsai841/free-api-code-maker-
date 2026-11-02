# APIForge — Free API Code Maker

APIForge is a free, browser-based tool for building HTTP requests and generating ready-to-use code snippets. Configure a request once, then generate it as **cURL**, **Python (requests)**, or **JavaScript (fetch)**. No account or installation is required for the core tool.

The project is a lightweight static website built with HTML, CSS, and vanilla JavaScript.

## Features

- Generate cURL, Python, and JavaScript request snippets.
- Support for GET, POST, PUT, PATCH, and DELETE methods.
- Add an optional Authorization value.
- Configure request headers and query parameters using JSON.
- Add a JSON request body for methods that support one.
- Copy the generated snippet.
- Load a sample POST request or reset the form.
- Optional AI helper for explaining or modifying generated requests using an OpenAI-compatible chat-completions endpoint and your own API key.
- Responsive layout for desktop and mobile.

## Use the website

Open the deployed website:

**https://premsai841.github.io/free-api-code-maker-/**

If GitHub Pages has not been enabled yet, follow the setup steps below.

## Run locally

The core application is a static page. You do not need Node.js, a package manager, or a build step.

1. Clone the repository:

   ```bash
   git clone https://github.com/premsai841/free-api-code-maker-.git
   ```

2. Open the project folder:

   ```bash
   cd free-api-code-maker-
   ```

3. Open `index.html` in a modern browser.

You can also serve the folder with a local static server if you prefer. There are no build commands or dependencies to install for the core generator.

## Deploy with GitHub Pages

1. Open the repository's **Settings** tab.
2. Select **Pages** from the sidebar.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Select branch **main** and folder **/(root)**.
5. Click **Save**.
6. Wait for GitHub Pages to publish the site. The URL should be:

   ```
   https://premsai841.github.io/free-api-code-maker-/
   ```

After future commits to the selected branch, GitHub Pages will publish the updated static files.

## How to use

1. Enter the full request URL, including `https://`.
2. Choose the HTTP method.
3. Add an Authorization value if the API requires one. For example: `Bearer YOUR_TOKEN`.
4. Add headers as a JSON object.
5. Add query parameters as a JSON object.
6. For POST, PUT, or PATCH requests, enter a valid JSON body when needed.
7. Select **Generate code**.
8. Switch between cURL, Python, and JavaScript tabs, then select **Copy snippet**.

### Example request

This example sends a JSON payload to JSONPlaceholder:

- **Method:** `POST`
- **URL:** `https://jsonplaceholder.typicode.com/posts`
- **Headers:**

  ```json
  {
    "Accept": "application/json"
  }
  ```

- **Query parameters:** `{}`
- **Body:**

  ```json
  {
    "title": "Hello API",
    "body": "Generated with APIForge",
    "userId": 1
  }
  ```

The generated snippets will make the equivalent request in each supported language. JSONPlaceholder is a demo API; it is useful for trying the example, not for storing production data.

## Optional AI helper

The AI helper can send your request details and a prompt to an OpenAI-compatible chat-completions endpoint.

1. Enter the endpoint URL.
2. Enter the model name supported by your provider.
3. Enter your own API key.
4. Describe what you want help with.
5. Select **Ask AI**.

**Security warning:** The AI request is sent directly from your browser to the endpoint you specify. This project does not provide a server-side proxy or secret vault. Use your own restricted key, understand your provider's billing and data policies, and do not enter a valuable or shared production secret into a public browser-based tool. The AI feature may also fail if the provider does not allow browser requests from this site's origin (CORS).

## Limitations

- This tool generates code; it does not execute the generated cURL, Python, or JavaScript request for you.
- The core generator accepts headers, query parameters, and request bodies as JSON. Invalid JSON must be corrected before code can be generated.
- The request body is omitted for GET and DELETE methods.
- Generated code is a starting point. Review it and add application-specific error handling, timeouts, retries, authentication handling, and secure secret management before production use.
- Python output uses the third-party `requests` package. Install it in your Python environment with `python -m pip install requests`.
- JavaScript output uses the browser-compatible `fetch` API. The target API may enforce CORS or other browser security restrictions.
- The AI helper requires a compatible provider, a valid key, network access, and provider support for browser-origin requests. Provider usage may incur charges.
- This is a static front-end project. It has no user accounts, backend, database, or server-side key protection.

## Tech stack

- HTML
- CSS
- Vanilla JavaScript
- Browser APIs: `fetch`, `URL`, and Clipboard API

## Contributing

Bug reports and improvements are welcome. For a change, fork the repository, create a branch, make your update, and open a pull request with a clear description of what changed and how you tested it.

## License

No license file is currently included. Unless a license is added to this repository, the code is not explicitly released under an open-source license. Check with the repository owner before reusing or redistributing it.
