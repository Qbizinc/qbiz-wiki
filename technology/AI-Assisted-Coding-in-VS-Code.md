---
title: AI Assisted Coding using VS Code
description: 
published: true
date: 2026-05-12T20:07:04.365Z
tags: 
editor: markdown
dateCreated: 2026-05-12T19:17:10.368Z
---

# Configuring VS Code for AI Assisted Coding
## Extensions
You need to use one of manuy available VS Extensions to expose a prompt and to configure a connection to one or more models.  I describe three extensions below:

### Continue
May have some connection with Cursor.  Install the Continue extension and use the settings button to configure access to your models.  This extension created a directory, under my home directory, named `~/.continue/config.yaml`.  The formate of this file is as follows:
```
name: Local Config
version: 1.0.0
schema: v1
models:
  - name: Autodetect
    provider: ollama
    model: AUTODETECT
  - name: Remote Ollama
    provider: ollama
    model: AUTODETECT
    apiBase: http://localhost:11435
  - name: Claude Sonnet
    provider: anthropic
    model: claude-sonnet-4-6
    apiKey: ...(your key here)...
  - name: Gemini 3 Pro
    provider: gemini
    model: gemini-3-pro-preview
    apiKey: ...(your key here)...
  - name: Gemini 3.1 Flash Pro Preview
    provider: gemini
    model: gemini-3.1-flash-lite-preview
    apiKey: ...(your key here)...
```
In the above, there are five model entries: two pointed at ollama, one on my local machine and another on a remote machine; one for Claud Sonnet, which uses a key I created in my Claude Pro account; finally, two different Gemini models, both using the same API key.

### CoPilot
Included with my GitHub Copilot Pro subscription, is limited access to various models (recently ChatGPT was removed from the offerings).  Install the **GitHub Copilot Chat** extension; pressing `Ctrl+I/Cmd+I` will open a chat prompt directly in the editor where you will see three tabs: **CLAUDE CODE**, **CHAT** and **CODEX**.

![vscode-copilot.jpg](/vscode-copilot.jpg)