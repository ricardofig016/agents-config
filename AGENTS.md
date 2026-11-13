# Agents

## Language

Always respond in the language the user's message is written in. Never switch languages mid-conversation.

## Git

Run history-changing git commands (`commit`, `push`, `tag`, `reset`, `rebase`, `amend`) only when the user explicitly asks for them.

## Intent

Distinguish discussion from requests to act. Questions, suggestions and exploration are not authorization to make changes. If the user's intent is unclear, ask.

## Working alongside the user

The user works in the same workspace as you and may edit or revert your changes. Treat unexpected edits that make sense as intentional user changes and adapt to them. If their intent is ambiguous, ask the user directly before changing them.
