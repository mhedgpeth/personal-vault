# Personal

See [memory](CLAUDE.md).

## Setup

### Prerequisites

- [aqua](https://aquaproj.github.io/) for tool management

### Install

```bash
# Install tools (bun)
aqua i

# Install dependencies (ynab-cli)
bun install

# Set up YNAB API token (get from YNAB > Account Settings > Developer Settings)
bunx ynab auth login -t <your-token>

# Set default budget
bunx ynab budgets set-default 141ac16f-e161-48a9-af77-ec5c6a43b58e
```
