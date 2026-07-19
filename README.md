# Strict AI Agent Governance Framework 🛡️

A production-grade system prompt designed to cure "AI Laziness", prevent superficial code generation, and enforce strict architectural hygiene for AI coding assistants .

## Why use this?
AI models often hallucinate test fixtures, use superficial search (`grep`), or delete code without understanding the full context. This prompt enforces a **Governance Framework** that forces the AI to act as a Principal Engineer.

## How to use
1. Copy the contents of `AI_SYSTEM_PROMPT.md`.
2. Paste it into your AI assistant's "System Prompt" or "Custom Instructions".
3. If you use **Cursor**, save it as a `.cursorrules` file in your project root.
4. If you use **Aider**, pass it as architectural guidelines.

## Core Features
- **No Superficial Text Search:** Forces AST analysis instead of blind `grep`.
- **Strict Test Fixtures:** Bans blind `MagicMock` and forces reading production `__init__`.
- **Deletion Lifecycle:** Prevents deleting code without a 5-step proof process.
