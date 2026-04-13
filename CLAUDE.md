# CLAUDE.md — Syncopate

This file provides guidance for AI assistants working in this repository.

## Project Overview

**Syncopate** is a web application for organizing West Coast Swing dance peer practice sessions. The core value propositions are:

- Role-balanced joining (Leaders vs. Followers)
- QR-code-based frictionless group entry
- Automated spotlight pairing via the Hungarian algorithm, minimizing repeat partnerships over a group's entire history

The full specification lives at `specs/initial-spec.md`.

## Current State

This repository is in the **specification phase**. No implementation code exists yet. The `specs/initial-spec.md` document is the authoritative source of truth for what needs to be built.

When implementation begins, all decisions about tech stack, directory structure, testing, and tooling should be recorded here.

## Key Domain Concepts

| Concept | Description |
|---|---|
| **Group** | Long-lived entity created by a registered user (the admin/coach). Persists across multiple sessions. |
| **Session** | A single practice event held under a Group. Initialized by the group owner. |
| **Dancer** | A participant in a session. Identified by a persistent browser cookie (no account required). |
| **Role** | Either `Leader` or `Follower`. Required at join time; may be re-confirmed each session. |
| **Spotlight** | A prompted pairing event triggered manually by the admin/coach. |
| **Pairing history** | Tracked at the **group level**, not per session. Persists across all sessions within the group so the algorithm always minimizes repeats over the group's full history. |

## Core Algorithm

The **Spotlight Pairing Generator** uses:

1. **Hungarian algorithm** (minimum cost bipartite matching) to pair Leaders with Followers
2. A **custom outer loop** to handle uneven group sizes (one role has more participants than the other)
3. **Cost function**: number of times a given Leader–Follower pair has danced together historically. Lower cost = preferred pairing.

When implementing, pairing history must be stored at the group level and queried efficiently before each spotlight generation.

## Identity & Authentication

- **Coaches/admins**: require explicit account registration
- **Dancers**: identified via a persistent browser cookie — no account creation required
- **Cookie loss recovery**: Coach generates a personalized, single-use recovery QR code from the dashboard. Scanning it re-links the device to the dancer's historical profile, keeping the pairing algorithm intact.

## User Flows Summary

### First Session
1. Admin creates a group → system generates QR code + join link
2. Dancers scan/click → enter lobby
3. Each dancer selects **Role** (required) and optionally enters a name (auto-alias if blank)
4. Admin triggers spotlight → pairings generated and displayed

### Recurring Sessions
1. Admin initializes a new session under the existing group
2. Returning dancers rejoin via cookie — name pre-filled, only role confirmation needed
3. Pairing history from all prior sessions informs the new spotlight pairings
4. Lost-cookie recovery flow available from the admin dashboard

## Development Guidelines (for when implementation begins)

### General Principles
- Prefer simple, explicit code over clever abstractions
- Do not add features, refactors, or "improvements" beyond what is explicitly requested
- Do not add error handling for scenarios that cannot happen
- Keep the pairing history scoped to the group — never per-session

### Adding a Tech Stack
When a framework/stack is chosen, update this file with:
- Language and runtime versions
- Framework and key dependencies
- How to run the dev server (`npm run dev`, `python manage.py runserver`, etc.)
- How to run tests
- How to run linting/formatting
- Database migration commands (if applicable)

### Commit Style
Follow conventional commits:
- `feat:` new feature
- `fix:` bug fix
- `docs:` documentation only
- `refactor:` code restructure without behavior change
- `test:` adding or updating tests
- `chore:` tooling, config, dependency updates

### Branch Naming
Feature branches follow the pattern `claude/<short-description>-<id>` (as seen in the git history).

## Repository Structure

```
syncopate/
├── specs/
│   └── initial-spec.md   # Full product specification (source of truth)
└── CLAUDE.md             # This file
```

## Git History

| Commit | Description |
|---|---|
| `0f2b766` | Merge: clarify pairing history scope |
| `f8c6421` | Clarify pairing history is scoped to group, not per session |
| `6ef0c55` | Merge: move spec to `specs/` directory |
| `3441db1` | Move specification doc to `specs/initial-spec.md` |
| `e611ae2` | Initial commit: add initial peer practice specification |
