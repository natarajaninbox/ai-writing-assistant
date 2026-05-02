# FAQ

## What do the action buttons do?

Highlight any text on a page to open the panel, then click an action:

| Button | What it does |
|---|---|
| **✦ Refine** | Rewrites and fixes all grammar/spelling errors, enhances clarity and readability — works on rough notes, copied web content, or informal writing |
| **↙ Shorten** | Reduces text to N words or fewer while keeping all key points (default 100 words; change in Settings → Shorten max words) |
| **≡ Summarize** | Condenses long text into a paragraph or bullet points (change style in Settings → Summarize style) |
| **? Explain** | Explains the text in simple language, breaking down complex concepts or technical terms |
| **✉ Chat** | Opens a chat window with the selected text as context — ask follow-up questions, explore ideas, or have a back-and-forth conversation |

After a result appears you can:
- **Copy** — copy result to clipboard
- **Swap** — move result back into the textarea for further iterations
- **Insert** — insert result into the last focused text field on the page

---

## How does the Chat feature work?

**Opening chat:** Click **✉ Chat** in the action row. The selected text appears as a yellow context bubble at the top of the chat window so the model always knows what you're referring to.

**Asking questions:** Type in the input box and press Enter (or Shift+Enter for a new line). The model streams its response token by token.

**Memory:** The full conversation history is kept in memory for the duration of the panel session. The model sees all previous messages, so you can ask follow-up questions naturally.

**Clearing chat:** Click the trash icon (top right of the chat toolbar) to wipe the conversation history and restore the original context bubble. Useful when starting a new line of questions about the same text.

**Closing the panel:** Pressing X or Escape clears all chat memory. The next time you select text and open Chat, it starts fresh.

**Going back:** Click **← Back** to return to the Refine / Shorten / Summarize / Explain view without losing your chat history (it resumes if you click Chat again).

---

## How to run with local models using Ollama

Ollama lets you run AI models entirely on your own machine — no API key, no cost, no data leaving your computer.

**Step 1 — Install Ollama**

Download and install from [ollama.com](https://ollama.com). On macOS it installs as a menu bar app.

**Step 2 — Pull a model**

Open Terminal and download a model:
```bash
ollama pull llama3.2
```
Popular free models: `llama3.2`, `llama3`, `deepseek-r1`, `mistral`, `phi3`, `gemma2`.

To see all models you have installed:
```bash
ollama list
```

**Step 3 — Start Ollama with CORS enabled**

Chrome extensions are blocked by Ollama's default CORS policy. You must start the server with origins open:

1. Quit Ollama completely from the menu bar (menu bar icon → Quit Ollama).
2. In Terminal, run:
   ```bash
   OLLAMA_ORIGINS="*" ollama serve
   ```
3. Keep that Terminal window open while using the extension.

> **Important:** `ollama run <model>` starts an interactive REPL — it does NOT start the server with the updated setting. You must use `ollama serve` with the env var above.

**Step 4 — Configure the extension**

1. Click the extension icon (or open the gear ⚙ inside the panel).
2. Set **Provider** to `Ollama (local, free)`.
3. **API Endpoint** auto-fills to `http://localhost:11434` — leave it as-is.
4. **API Key** is hidden — Ollama doesn't require one.
5. Set **Model / Deployment** to the model you pulled (e.g. `llama3.2`), or click a chip.
6. Click **Save Settings**, then **Test** to verify.

---

## Ollama — HTTP 403 / CORS blocked

**Symptom:** Clicking **Test** or running any action with Ollama selected shows `✗ Ollama CORS error (403)`.

**Cause:** Chrome extensions send an `Origin: chrome-extension://...` header with every request. Ollama's default CORS policy only allows localhost origins and rejects this, returning 403. This cannot be bypassed from the extension side — the fix must be applied to Ollama.

**Root cause:** Chrome adds an `Origin: chrome-extension://...` header to all extension requests. Ollama's default CORS policy rejects this. There is no workaround from the extension side — the fix must be applied to Ollama.

**Fix — most reliable (works every time):**

1. Quit Ollama completely from the menu bar (menu bar icon → Quit Ollama).
2. In Terminal, run:
   ```bash
   OLLAMA_ORIGINS="*" ollama serve
   ```
3. Keep that Terminal window open while using the extension.

> **Important:** Running `ollama` or `ollama run <model>` in a separate terminal is the interactive REPL — it does NOT start a new server with the updated setting. You must use `ollama serve` with the env var, and the menu bar app must be fully quit first.

**Permanent fix (survives reboots — macOS):**

Option A — launchctl (apply before restarting Ollama app):
```bash
launchctl setenv OLLAMA_ORIGINS "*"
```
Then quit Ollama from menu bar and reopen it. Repeat after every system reboot.

Option B — create a wrapper launch script (run once):
```bash
mkdir -p ~/bin
echo '#!/bin/bash\nOLLAMA_ORIGINS="*" ollama serve' > ~/bin/ollama-serve.sh
chmod +x ~/bin/ollama-serve.sh
```
Add `~/bin/ollama-serve.sh` to your login items (System Settings → General → Login Items) or run it manually from Terminal instead of the menu bar app.

---

## Ollama — which models are available?

Run in Terminal:
```bash
ollama list
```

To install a new model:
```bash
ollama pull llama3.2
```

Popular free models: `llama3.2`, `llama3`, `deepseek-r1`, `mistral`, `phi3`, `gemma2`.

---

## Azure OpenAI — Forbidden / 403

**Symptom:** Error bar shows `Forbidden` when running an action.

**Cause:** Usually one of:
- API key is incorrect or expired
- The deployment name doesn't match what's configured in Azure
- The endpoint URL has a typo

**Fix:** Open the settings gear, verify all three fields match your Azure resource exactly, then click **Save Settings** — the validator will confirm connectivity.

---

## Extension context invalidated

**Symptom:** Error bar shows `Extension was reloaded — please refresh this page.`

**Cause:** The extension was reloaded (via `chrome://extensions`) while the content script was still active in an open tab.

**Fix:** Refresh the tab.

---

## Insert button is greyed out

**Symptom:** The **Insert** button is disabled after getting a result.

**Cause:** Insert requires a text field to have been focused *before* you selected text to open the panel. The target is captured at panel-open time.

**Fix:** Click into a text field or textarea first, then select text elsewhere on the page. The Insert button will be enabled.
