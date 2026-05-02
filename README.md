# AI Writing Assistant — Chrome Extension

A floating Chrome extension that appears whenever you highlight text on any webpage. Run actions on the selected text or open a chat window to ask questions — all powered by your choice of AI provider.

---

## Features

### Actions
Highlight any text → panel opens → choose an action:

| Button | What it does |
|---|---|
| **✦ Refine** | Rewrites and fixes errors, improves clarity — works on rough notes or copied web content |
| **↙ Shorten** | Reduces to a configured word limit while preserving key points |
| **≡ Summarize** | Condenses into a paragraph or bullet points (configurable) |
| **? Explain** | Explains in plain language, breaks down jargon and complex terms |
| **✉ Chat** | Opens a chat window with the selected text as context for Q&A |

### Chat
- Selected text is pre-loaded as context so the model knows what you're asking about
- Full conversation memory within the panel session
- Trash icon clears history and restores context
- Enter to send, Shift+Enter for new line

### Result actions
After any action produces a result:
- **Copy** — copy to clipboard
- **Swap** — move result back into the textarea for further iteration
- **Insert** — insert into the last focused text field on the page

### Panel behaviour
- **Floating** — panel stays open when clicking outside; close with X or Escape
- **Draggable** — drag the header to reposition anywhere on screen
- **Position memory** — reopens at the same location on next use (saved to localStorage)
- **Expand** — widen the panel for longer text (toggle in header)

---

## Supported Providers

| Provider | Cost | Requires API key |
|---|---|---|
| Azure OpenAI | Paid | Yes |
| OpenAI | Paid | Yes |
| Anthropic (Claude) | Paid | Yes |
| Ollama | Free — runs locally | No |
| OpenRouter | Free tier available | Yes |
| Groq | Free tier available | Yes |
| HuggingFace | Free tier available | Yes |

---

## Installation

1. Go to `chrome://extensions`
2. Enable **Developer mode** (top right toggle)
3. Click **Load unpacked** → select this directory
4. The extension icon appears in the toolbar

After reloading the extension, refresh any tab where it was already active.

---

## Configuration

Open the settings panel via the **⚙ gear icon** inside the floating panel, or click the extension icon in the Chrome toolbar.

| Field | Description |
|---|---|
| Provider | Select your AI provider |
| API Endpoint | Auto-fills for most providers; Azure requires your resource URL |
| API Key | Your provider API key (hidden for Ollama) |
| Model / Deployment | Model name or Azure deployment name |
| Shorten — max words | Word limit for the Shorten action (default: 100) |
| Summarize style | Paragraph or bullet points |

Click **Save Settings** to validate connectivity, then **Test** to send a real question to the model.

Model name suggestions appear as clickable chips below the model field when a provider is selected.

---

## Using Ollama (local, free)

Run AI models entirely on your own machine — no API key, no cost, no data sent externally.

**1. Install Ollama:** [ollama.com](https://ollama.com)

**2. Pull a model:**
```bash
ollama pull llama3.2
```

**3. Start with CORS enabled** (required for Chrome extensions):
```bash
# Quit Ollama from the menu bar first, then:
OLLAMA_ORIGINS="*" ollama serve
```
Keep that terminal open while using the extension.

**4. Configure:** Set Provider to `Ollama (local, free)`, endpoint stays `http://localhost:11434`, enter your model name.

> See [FAQ.md](FAQ.md) for the full Ollama setup guide and CORS troubleshooting.

---

## Keyboard shortcuts

| Key | Action |
|---|---|
| Escape | Close the panel |
| Enter (in chat) | Send message |
| Shift+Enter (in chat) | New line |

---

## Files

```
manifest.json     — MV3 manifest, permissions, host declarations
background.js     — Service worker: builds requests, streams SSE, handles all providers
content.js        — Injected into every page: panel UI, drag, chat, all interactions
content.css       — Panel styles (scoped to #ai-assist-panel)
popup.html/js     — Toolbar popup for configuring credentials
models.json       — Model name suggestions per provider
icons/            — 16/48/128px extension icons
FAQ.md            — Troubleshooting and how-to guides
```

---

## Troubleshooting

See [FAQ.md](FAQ.md) for:
- Ollama CORS 403 error and fix
- Azure Forbidden / 403
- Insert button greyed out
- Extension context invalidated
