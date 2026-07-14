# Contributing to AARA Prep

Thank you for your interest in contributing to AARA Prep. Contributions can include bug fixes, documentation, accessibility improvements, tests, and carefully scoped product features.

## Before you start

1. Read the README and roadmap.
2. Search existing issues before opening a new one.
3. For substantial changes, open an issue first.
4. Never commit secrets, API keys, private user data, or real mental-health information.

## Local setup

Requirements: Node.js 18 or later and npm. Store development credentials in `.env.local`.

Install dependencies with `npm install` and run the app with `npm run dev`.

Useful checks are `npm run lint`, `npm run typecheck`, and `npm run build`.

Some features require external Firebase, OpenAI, Stripe, or ElevenLabs configuration. Document setup limitations in your pull request.

## Pull requests

A good pull request explains what changed and why, links a related issue when available, includes screenshots for visual changes, lists checks run, updates documentation when needed, and avoids unrelated changes.

By contributing, you agree to follow the Code of Conduct.
