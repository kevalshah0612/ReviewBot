# ReviewBot

Multi-Agent Code Review Orchestrator for GitHub pull requests.

## Overview

ReviewBot is a LangGraph-powered multi-agent system that automatically reviews GitHub pull requests. It uses specialized agents to inspect architecture, security, style, and test coverage, then posts a structured review comment back to the PR.

The system is designed to behave more like an engineering workflow than a single LLM prompt.

## Why this project

Manual code review takes time, creates bottlenecks, and can be inconsistent across reviewers. ReviewBot helps teams get faster and more consistent feedback by running automated multi-agent review on every pull request.

This project is useful because it demonstrates:
- Multi-agent orchestration
- Tool-using LLM workflows
- Persistent memory with Redis
- GitHub webhook and API integration
- CI/CD-ready deployment patterns

## Features

- GitHub webhook listener for pull request events
- 5 specialized review agents
- Structured review summaries posted back to GitHub
- Redis-based short-term and long-term memory
- Security scanning with Bandit
- Style analysis with pylint
- FastAPI service for webhook handling
- Dockerized local deployment
- GitHub Actions integration

## Agents

1. ArchitectureAgent  
   Reviews coupling, abstractions, and design patterns

2. SecurityAgent  
   Checks for security risks such as secrets, injection, and auth issues

3. StyleAgent  
   Reviews naming, formatting, and complexity

4. TestCoverageAgent  
   Flags code changes without sufficient tests

5. SummaryAgent  
   Aggregates findings into a clean markdown review comment

## Architecture

```text
GitHub Pull Request Event
          │
          ▼
[FastAPI Webhook]
          │
          ▼
[LangGraph Orchestrator]
  ├── ArchitectureAgent
  ├── SecurityAgent
  ├── StyleAgent
  ├── TestCoverageAgent
  └── SummaryAgent
          │
          ▼
[Redis Memory]
  ├── Short-term PR state
  └── Long-term repo patterns
          │
          ▼
[GitHub API Comment on PR]
```

## Tech Stack

- Python
- LangGraph
- Redis
- FastAPI
- Uvicorn
- Docker
- GitHub Actions
- OpenAI API
- httpx
- Bandit
- pylint

## Project Structure

```text
reviewbot/
├── README.md
├── agents/
│   ├── architecture.py
│   ├── security.py
│   ├── style.py
│   ├── test_coverage.py
│   └── summary.py
├── orchestrator.py
├── memory.py
├── github_service.py
├── run_review.py
├── api/
│   └── main.py
├── results/
│   ├── latency_log.csv
│   ├── latency_summary.json
│   └── review_cycle_reduction.json
├── .github/
│   └── workflows/
│       └── review.yml
├── Dockerfile
├── docker-compose.yml
└── requirements.txt
```

## Getting Started

### Prerequisites

- Python 3.11
- Redis
- Docker and Docker Compose
- OpenAI API key
- GitHub token

### Install

```bash
git clone https://github.com/your-username/reviewbot.git
cd reviewbot
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Run with Docker

```bash
docker-compose up --build
```

### Run webhook API

```bash
uvicorn api.main:app --host 0.0.0.0 --port 8000
```

## Workflow

1. GitHub sends a pull request webhook
2. ReviewBot fetches PR metadata and file diffs
3. LangGraph runs specialized review agents
4. Redis stores per-PR state and cross-PR patterns
5. ReviewBot posts a structured comment back to GitHub

## Metrics

Planned metrics include:
- 5-agent orchestration graph
- 40% fewer human review cycles
- 200+ PRs processed through replay and testing
- Sub-3s response time target per agent invocation

## Roadmap

- [ ] Build GitHub integration layer
- [ ] Create LangGraph multi-agent workflow
- [ ] Add Redis short-term and long-term memory
- [ ] Build FastAPI webhook service
- [ ] Add GitHub Actions workflow
- [ ] Replay historical pull requests
- [ ] Publish measured metrics in `results/`

## Demo

TODO:
- Add architecture diagram
- Add screenshot of PR review comment
- Add webhook demo GIF
- Add metrics table with real numbers

## Related Projects

- FilingQuery — source of replay/testing repos and shared AI engineering patterns
- EvalTrace — evaluation layer for AI systems

## License

MIT
