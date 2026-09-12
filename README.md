# Agent Sandbox

Run an agent in an isolated sandbox with Docker sandboxes.

The kit spec installs tools that the agent can make use of such as linters, and bundles config files for different models.

## Usage

The sandbox kit is of type `mixin` so can be run with different agent models at runtime whilst still loading all the config files that work with different agents.

Using a sandbox kit locks me to one image, whereas a mixin means I can be flexible in choosing a different agent tool depending on my use case:

**Run a claude sandbox**
```sh
sbx run claude --kit .
```

**Run a Kiro sandbox**
```sh
sbx run kiro --kit .
```
