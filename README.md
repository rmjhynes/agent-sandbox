# Agent Sandbox

Run an agent in an isolated sandbox with Docker sandboxes.

The kit spec installs tools that are useful to the agent such as linters, and bundles config files for different models.

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

## Kit Layers

Kits can be layered so that repo specific kits can inherit configuration from a 'base' kit which installs and configures common utilties. The kit in this repo is configured to act as the base for other repositories.

### Example

If I had a Python repository which needed to make use of tools such as `uv`, they could be installed in `~/repos/python-project/spec.yaml`.

The Docker sandbox would then have utilities from the base kit such as `jq` as well as python specific tooling like `uv`.

```
Host
├── ~/kits/agent-sandbox/
│   └── spec.yaml                                           # base kit
│
├── ~/repos/python-project/                                 # example python repo
│   ├── spec.yaml                                           # repo-specific kit
│   │   └── files/workspace/.claude/skills/python-skill
│   │       └── SKILL.md                                    # claude skill file
│   └── ...
│
└── Sandbox (microVM)
    ├── Private Docker daemon
    ├── /home/agent/.claude/skills                          # claude skills files
    ├── /home/agent/repos/python-project/                   # workspace bind-mounted from host
    └── ...
```

Layer the python specific kit on top of the base kit:
```sh
cd ~/repos/python-project
sbx run claude --kit ~/kits/agent-sandbox/ --kit .
```
