# showboat

A Claude Code plugin for creating executable demo documents using [showboat](https://github.com/nichochar/showboat) and [rodney](https://github.com/nichochar/rodney).

## What it does

Teaches Claude how to combine two CLI tools to create polished product demos:

- **showboat** - Assembles markdown documents with embedded code blocks, captured output, and screenshots
- **rodney** - Automates Chrome for clicking, typing, navigating, and taking screenshots

## Prerequisites

Both CLI tools must be installed and on your PATH:

```bash
# Verify installation
showboat --help
rodney --help
```

The target web application must be running and accessible before creating a demo.

## Installation

```bash
claude --plugin-dir /path/to/showboat-plugin
```

Or add to your project's `.claude/plugins` configuration.

## Usage

Ask Claude to create a demo:

- "Create a demo of the login flow"
- "Make a walkthrough of the new feature with screenshots"
- "Use showboat and rodney to document the onboarding process"

## Plugin Structure

```
showboat-plugin/
  .claude-plugin/
    plugin.json
  skills/
    demo-creation/
      SKILL.md                      # Main skill (workflow + patterns)
      references/
        rodney-commands.md           # Full rodney CLI reference
        showboat-commands.md         # Full showboat CLI reference
      examples/
        demo-workflow.md             # Annotated example session
  README.md
```
