# Showboat Plugin for Claude Code

A Claude Code plugin that teaches Claude how to create executable demo documents using [showboat](https://github.com/nichochar/showboat) (document assembly) and [rodney](https://github.com/nichochar/rodney) (browser automation).

## What It Does

This plugin adds a **demo-creation** skill that lets you ask Claude to build reproducible markdown walkthroughs. Claude will:

- Plan a demo structure with narrative text and screenshots
- Drive a browser with rodney (click, type, navigate, wait)
- Capture screenshots at each step via `showboat image`
- Assemble everything into a single executable markdown document with `showboat`
- Recover from errors using `showboat pop` and retry

## Prerequisites

Install the CLI tools:

- [showboat](https://github.com/nichochar/showboat) - markdown document assembly
- [rodney](https://github.com/nichochar/rodney) - Chrome browser automation

## Installation

```shell
# Add the marketplace
/plugin marketplace add snopoke/showboat-claude-plugin

# Install the plugin
/plugin install showboat@snopoke-showboat-claude-plugin
```

## Usage

Once installed, ask Claude to create a demo:

- "Create a demo of the login flow"
- "Make a walkthrough of the dashboard with screenshots"
- "Document the signup workflow using showboat"
- "Record a demo of the settings page"

Claude will use `rodney start` to launch a browser, `showboat init` to create the document, then alternate between narrative text, browser actions, and screenshots to build the full demo.

## Plugin Structure

```
plugins/showboat/
  .claude-plugin/
    plugin.json               # Plugin manifest
  skills/
    demo-creation/
      SKILL.md                # Core skill: demo creation workflow
      references/
        showboat-commands.md  # Showboat CLI tips
        rodney-commands.md    # Rodney CLI tips
      examples/
        demo-workflow.md      # Annotated example session
```

## License

MIT
