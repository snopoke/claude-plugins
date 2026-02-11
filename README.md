# snopoke's Claude Code Plugin Marketplace

A collection of Claude Code plugins.

## Plugins

| Plugin | Description |
|--------|-------------|
| [showboat](plugins/showboat/) | Create executable demo documents using [showboat](https://github.com/nichochar/showboat) and [rodney](https://github.com/nichochar/rodney) CLI tools |

## Installation

Add the marketplace, then install individual plugins:

```shell
# Add the marketplace
/plugin marketplace add snopoke/showboat-claude-plugin

# Install a plugin
/plugin install showboat@snopoke-showboat-claude-plugin
```

## Repository Structure

```
.claude-plugin/
  marketplace.json          # Marketplace catalog listing all plugins
plugins/
  showboat/                 # Each plugin in its own directory
    .claude-plugin/
      plugin.json
    skills/
      ...
```
