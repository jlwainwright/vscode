# Gemini-Code Architecture

This document outlines the proposed architectural approach for integrating multiple AI SDKs into a fork of Visual Studio Code, creating "Gemini-Code". The architecture aims for deep integration, performance, and extensibility.

## 1. High-Level Overview

Gemini-Code will be a modified version of the VS Code codebase. The AI agents (gemini-cli, claude-code-sdk, openai-agent-sdk) will run as separate processes or be accessed via API, communicating with the VS Code frontend and backend components through a model router abstraction layer.

## 2. Key Architectural Components

### 2.1. VS Code Frontend (UI)
* AI chat panel, context menus, command palette, and inline code suggestions.
* Communicates with backend via IPC.

### 2.2. VS Code Backend / Extension Host
* Manages lifecycle of all AI processes.
* Routes requests to `gemini-cli`, `claude-code-sdk`, or `openai-agent-sdk` via API abstraction.
* Provides context (file, selection, project info).

### 2.3. AI Agent Layer (CLI/API Wrappers)
* Includes:
  - `gemini-cli`
  - `claude-code-sdk`
  - `openai-agent-sdk`
* Exposes a unified interface to the backend.

### 2.4. AI Router Module
* Abstracts calls to different models.
* Supports fallback logic and config-driven selection.

## 3. Communication Flow
1. User triggers action (e.g. "Explain Code")
2. Frontend sends context to backend
3. Backend routes via AI Router to preferred provider
4. Model responds, data is sent back and displayed

## 4. Development Considerations
* Performance, extensibility, and secure model communication
* Ability to plug in future models (e.g. Mistral, LLaMA)