# Copilot CLI Plugin Marketplace

Welcome to the **GitHub Copilot CLI Plugin Marketplace** demo repository. This project showcases a complete plugin marketplace for GitHub Copilot CLI, featuring three example plugins that demonstrate how to extend and customize Copilot's capabilities. Use this repository as a reference for discovering, installing, and developing plugins.

## Prerequisites

Before you get started, ensure you have:

- **GitHub Copilot CLI** installed ([installation guide](https://docs.github.com/en/copilot/how-tos/copilot-cli/install-the-copilot-cli))
- An active **GitHub Copilot subscription**
- **Git** (for cloning the marketplace locally, optional)

## Quick Start

Get up and running with plugins in three steps:

**1. Register the Marketplace**
```sh
copilot plugin marketplace add OWNER/ghcp-lab
```

**2. Browse Available Plugins**
```sh
copilot plugin marketplace browse ghcp-lab-marketplace
```

**3. Install a Plugin**
```sh
copilot plugin install alert-toolkit@ghcp-lab-marketplace
```

That's it! Your plugins are now ready to use.

## Registering This Marketplace

The marketplace is registered as a remote source that makes all plugins discoverable and installable. Use the `copilot plugin marketplace add` command with either a GitHub repository reference or a local path.

### From GitHub
```sh
copilot plugin marketplace add OWNER/ghcp-lab
```

This registers the marketplace using the GitHub repository URL. Copilot CLI will look for the marketplace configuration in the `.github/plugin/` directory.

### From a Local Clone
```sh
# Clone the repository first
git clone https://github.com/OWNER/ghcp-lab.git
cd ghcp-lab

# Register the local marketplace
copilot plugin marketplace add ./
```

After registration, the marketplace is identified as `ghcp-lab-marketplace` in plugin commands.

## Browsing Available Plugins

View all available plugins in the marketplace:

```sh
copilot plugin marketplace browse ghcp-lab-marketplace
```

This displays a list of all plugins, including their names, versions, and descriptions. The marketplace currently includes three example plugins:

| Plugin | Version | Description |
|--------|---------|-------------|
| **alert-toolkit** | v1.0.0 | Alert analysis and triage tools for security event processing |
| **code-reviewer** | v1.2.0 | Code review assistant with security and quality checks |
| **learning-companion** | v0.5.0 | Learning companion for discovering Copilot CLI features |

## Installing Plugins

Plugins can be installed from the registered marketplace, directly from GitHub, or from a local directory. Choose the method that works best for your workflow.

### From the Registered Marketplace
```sh
copilot plugin install alert-toolkit@ghcp-lab-marketplace
copilot plugin install code-reviewer@ghcp-lab-marketplace
copilot plugin install learning-companion@ghcp-lab-marketplace
```

### Directly from GitHub
```sh
copilot plugin install OWNER/ghcp-lab:plugins/alert-toolkit
copilot plugin install OWNER/ghcp-lab:plugins/code-reviewer
copilot plugin install OWNER/ghcp-lab:plugins/learning-companion
```

### From a Local Clone
```sh
copilot plugin install ./plugins/alert-toolkit
copilot plugin install ./plugins/code-reviewer
copilot plugin install ./plugins/learning-companion
```

## Managing Plugins

Once installed, manage your plugins with these commands:

| Command | Description |
|---------|-------------|
| `copilot plugin list` | List all installed plugins and their status |
| `copilot plugin update NAME` | Update a specific plugin to the latest version |
| `copilot plugin update --all` | Update all installed plugins at once |
| `copilot plugin disable NAME` | Temporarily disable a plugin without removing it |
| `copilot plugin enable NAME` | Re-enable a previously disabled plugin |
| `copilot plugin uninstall NAME` | Remove a plugin completely |

## Verifying Installation

After installing plugins, verify they are properly loaded and available for use.

### Check Installation Status
```sh
copilot plugin list
```

This shows all installed plugins with their versions and status (enabled/disabled).

### In an Interactive Session
Start a Copilot CLI session and use these commands to confirm everything is working:

```sh
# List available plugins in the session
/plugin list

# List available agents (including those from plugins)
/agent

# List available skills (including those from plugins)
/skills list
```

You should see your newly installed plugins appear in the output.

## Plugin Details

### alert-toolkit

The alert-toolkit plugin provides specialized tools for analyzing, classifying, and triaging security alerts and events. Perfect for security engineers and incident responders.

**What it includes:**
- **alert-analyst** agent — Analyzes alert data and provides insights
- **triage-alerts** skill — Classifies and prioritizes alerts by severity

**Example usage:**
```
# In a Copilot CLI session
Analyze the alert logs for patterns

# Use the skill directly
/triage-alerts from recent incidents
```

### code-reviewer

The code-reviewer plugin enhances your code review workflow with AI-powered assistance. Use it to identify potential issues, security vulnerabilities, and best practice violations before merging.

**What it includes:**
- **reviewer** agent — Reviews code changes and provides feedback
- **review-checklist** skill — Generates language-specific review checklists

**Example usage:**
```
# In a Copilot CLI session
Review my staged changes for security issues

# Use the skill directly
/review-checklist TypeScript
```

### learning-companion

The learning-companion plugin is designed to help users learn and master GitHub Copilot CLI features through guided lessons and interactive exercises.

**What it includes:**
- **tutor** agent — Teaches Copilot CLI concepts and best practices
- **practice-exercise** skill — Generates interactive exercises for skill building

**Example usage:**
```
# In a Copilot CLI session
Explain how inline chat differs from the chat panel

# Use the skill directly
/practice-exercise completions beginner
```

## Marketplace Architecture

The marketplace is organized around a centralized configuration file that defines all available plugins and their locations.

### Structure
```
.github/
  plugin/
    marketplace.json          # Central marketplace configuration

plugins/
  alert-toolkit/              # Example plugin directory
    plugin.json               # Plugin manifest (required)
    agents/
      alert-analyst.agent.md  # Agent definition
    skills/
      triage-alerts/
        SKILL.md              # Skill definition
  code-reviewer/              # Example plugin directory
    ...
  learning-companion/         # Example plugin directory
    ...
```

### marketplace.json
The `marketplace.json` file (located in `.github/plugin/`) defines all plugins available in this marketplace. Each entry specifies:
- Plugin name and version
- Description
- Source path to the plugin directory (relative to the repo root)
- Keywords and category for discovery

Example entry:
```json
{
  "plugins": [
    {
      "name": "alert-toolkit",
      "version": "1.0.0",
      "description": "Alert analysis and triage tools",
      "source": "plugins/alert-toolkit"
    }
  ]
}
```

### Loading and Precedence
Copilot CLI uses the following precedence order:
- **Agents and skills**: First-found-wins. Project-level agents/skills (`.github/agents/`, `.github/skills/`) take precedence over plugin-provided ones.
- **MCP servers**: Last-wins. Plugin MCP configurations can override earlier ones.
- **Built-in tools and agents** are always present and cannot be overridden.

## Creating Your Own Plugin

Want to build a custom plugin? Follow these steps to create and integrate your own:

### Step 1: Create the Plugin Directory
```sh
mkdir my-plugin
cd my-plugin
```

### Step 2: Create plugin.json
Define your plugin's manifest:
```json
{
  "name": "my-plugin",
  "description": "My custom Copilot plugin",
  "version": "1.0.0",
  "author": {
    "name": "Your Name"
  },
  "license": "MIT",
  "agents": "agents/",
  "skills": "skills/"
}
```

### Step 3: Add Agents and Skills
Create subdirectories and define your custom agents and skills:
```
my-plugin/
  plugin.json
  agents/
    my-agent.agent.md     # Agent definition (markdown with YAML frontmatter)
  skills/
    my-skill/
      SKILL.md            # Skill definition (markdown with YAML frontmatter)
```

### Step 4: Test Locally
Install your plugin from the local directory:
```sh
copilot plugin install ./my-plugin
```

Test it in a Copilot CLI session to ensure agents and skills work as expected.

### Step 5: Publish to Marketplace
Add your plugin to this marketplace by including it in `marketplace.json`:
```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "My custom Copilot plugin",
  "source": "plugins/my-plugin"
}
```

Then share your repository with others to make it discoverable!

### Learn More
For comprehensive plugin development documentation, visit:
- [Creating a Plugin for GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating)
- [GitHub Copilot CLI Plugin Reference](https://docs.github.com/en/copilot/reference/cli-plugin-reference)

## Further Reading

Explore these resources to deepen your understanding of Copilot CLI plugins and marketplaces:

- **[Creating Custom Plugins](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating)** — Full guide to building your own plugins
- **[Plugin Marketplaces](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-marketplace)** — How marketplaces work and how to create one
- **[Finding and Installing Plugins](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-finding-installing)** — Discover and install community plugins
- **[Copilot CLI Plugin Reference](https://docs.github.com/en/copilot/reference/cli-plugin-reference)** — Complete command and configuration reference

---

**Ready to get started?** Begin with the [Quick Start](#quick-start) section above, or explore the example plugins in the `plugins/` directory to see how they're structured.
