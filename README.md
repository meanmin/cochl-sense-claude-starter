# Cochl.Sense API Integration Skill for Claude

A Claude Code skill that transforms Claude into a **Cochl.Sense Cloud API integration expert**, enabling rapid implementation of AI-powered audio event detection in your applications.

## What is This Skill?

This skill equips Claude with comprehensive knowledge and best practices for integrating the Cochl.Sense Cloud API. When activated, Claude becomes an expert assistant that can:

- Guide you through secure API setup with proper key management
- Generate production-ready code following security best practices
- Handle Python environment configuration and dependency management
- Troubleshoot common integration issues automatically
- Implement audio analysis features quickly and safely

## What Claude Can Do With This Skill

When this skill is active, Claude can help you:

- **Set Up API Access**: Guide API key acquisition from Cochl Dashboard and configure secure `.env` files
- **Install Dependencies**: Set up Python environments and handle package installation correctly
- **Write Secure Code**: Generate production-ready code with proper validation and error handling
- **Parse SDK Responses**: Extract sound tags, probabilities, and timestamps correctly from API responses
- **Implement Advanced Features**: Batch processing, custom sensitivity thresholds, and demo modes
- **Troubleshoot Issues**: Diagnose and fix common dependency, authentication, and integration problems

## Installation

### For Claude Code CLI

```bash
# Navigate to your project directory
cd cochl-sense-claude-starter

# The skill is already included in .claude/skills/cochlAPI/
# Claude will automatically detect and offer to use it when relevant
```

### For Claude.ai

Upload the `.claude/skills/cochlAPI/` folder to your Claude.ai project to enable this skill.

## Usage

Claude will automatically activate this skill when you mention Cochl API-related tasks:

- "Help me integrate the Cochl API into my Flask app"
- "I need to add sound detection to my Python application"
- "How do I analyze audio files with the Cochl API?"

All implementation details, security requirements, and troubleshooting steps are defined in `.claude/skills/cochlAPI/SKILL.md`.

## Demo Application

This repository includes a simple Flask demo app (`app.py`) that demonstrates the skill's capabilities. Ask Claude to help you run it:

"Help me run the demo Flask application"

Claude will guide you through the setup process using the knowledge from this skill.

## Related Links

- [Cochl Dashboard](https://dashboard.cochl.ai) - Get your API key
- [Cochl Official Documentation](https://docs.cochl.ai/sense/cochl.sense-cloud-api/gettingstarted/) - API reference
- [Claude Skills Documentation](https://code.claude.com/docs/en/skills) - Learn about Claude Code skills

## License

This skill is provided for demonstration and educational purposes.
