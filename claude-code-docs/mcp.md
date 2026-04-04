# Claude Code - MCP (Model Context Protocol) & Integrations

Documentation for connecting Claude Code to external tools and services via MCP, channels, and third-party integrations.

## Core MCP Documentation

- [Connect Claude Code to tools via MCP](https://code.claude.com/docs/en/mcp.md): Learn how to connect Claude Code to your tools with the Model Context Protocol.
- [Push events into a running session with channels](https://code.claude.com/docs/en/channels.md): Use channels to push messages, alerts, and webhooks into your Claude Code session from an MCP server. Forward CI results, chat messages, and monitoring events so Claude can react while you're away.
- [Channels reference](https://code.claude.com/docs/en/channels-reference.md): Build an MCP server that pushes webhooks, alerts, and chat messages into a Claude Code session. Reference for the channel contract: capability declaration, notification events, reply tools, sender gating, and permission relay.
- [Extend Claude Code](https://code.claude.com/docs/en/features-overview.md): Understand when to use CLAUDE.md, Skills, subagents, hooks, MCP, and plugins.

## Plugins & Marketplaces

- [Create plugins](https://code.claude.com/docs/en/plugins.md): Create custom plugins to extend Claude Code with skills, agents, hooks, and MCP servers.
- [Plugins reference](https://code.claude.com/docs/en/plugins-reference.md): Complete technical reference for Claude Code plugin system, including schemas, CLI commands, and component specifications.
- [Discover and install prebuilt plugins through marketplaces](https://code.claude.com/docs/en/discover-plugins.md): Find and install plugins from marketplaces to extend Claude Code with new commands, agents, and capabilities.
- [Create and distribute a plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces.md): Build and host plugin marketplaces to distribute Claude Code extensions across teams and communities.

## Third-Party Service Integrations

- [Claude Code GitHub Actions](https://code.claude.com/docs/en/github-actions.md): Learn about integrating Claude Code into your development workflow with Claude Code GitHub Actions.
- [Claude Code with GitHub Enterprise Server](https://code.claude.com/docs/en/github-enterprise-server.md): Connect Claude Code to your self-hosted GitHub Enterprise Server instance for web sessions, code review, and plugin marketplaces.
- [Claude Code GitLab CI/CD](https://code.claude.com/docs/en/gitlab-ci-cd.md): Learn about integrating Claude Code into your development workflow with GitLab CI/CD.
- [Claude Code in Slack](https://code.claude.com/docs/en/slack.md): Delegate coding tasks directly from your Slack workspace.
- [Code Review](https://code.claude.com/docs/en/code-review.md): Set up automated PR reviews that catch logic errors, security vulnerabilities, and regressions using multi-agent analysis of your full codebase.
- [Orchestrate teams of Claude Code sessions](https://code.claude.com/docs/en/agent-teams.md): Coordinate multiple Claude Code instances working together as a team, with shared tasks, inter-agent messaging, and centralized management.

## Cloud Provider Integrations

- [Claude Code on Amazon Bedrock](https://code.claude.com/docs/en/amazon-bedrock.md): Learn about configuring Claude Code through Amazon Bedrock, including setup, IAM configuration, and troubleshooting.
- [Claude Code on Google Vertex AI](https://code.claude.com/docs/en/google-vertex-ai.md): Learn about configuring Claude Code through Google Vertex AI, including setup, IAM configuration, and troubleshooting.
- [Claude Code on Microsoft Foundry](https://code.claude.com/docs/en/microsoft-foundry.md): Learn about configuring Claude Code through Microsoft Foundry, including setup, configuration, and troubleshooting.
- [LLM gateway configuration](https://code.claude.com/docs/en/llm-gateway.md): Learn how to configure Claude Code to work with LLM gateway solutions. Covers gateway requirements, authentication configuration, model selection, and provider-specific endpoint setup.

## Enterprise & Infrastructure

- [Enterprise deployment overview](https://code.claude.com/docs/en/third-party-integrations.md): Learn how Claude Code can integrate with various third-party services and infrastructure to meet enterprise deployment requirements.
- [Enterprise network configuration](https://code.claude.com/docs/en/network-config.md): Configure Claude Code for enterprise environments with proxy servers, custom Certificate Authorities (CA), and mutual Transport Layer Security (mTLS) authentication.
- [Development containers](https://code.claude.com/docs/en/devcontainer.md): Learn about the Claude Code development container for teams that need consistent, secure environments.
- [Monitoring](https://code.claude.com/docs/en/monitoring-usage.md): Learn how to enable and configure OpenTelemetry for Claude Code.
- [Track team usage with analytics](https://code.claude.com/docs/en/analytics.md): View Claude Code usage metrics, track adoption, and measure engineering velocity in the analytics dashboard.
- [Schedule recurring tasks in Claude Code Desktop](https://code.claude.com/docs/en/desktop-scheduled-tasks.md): Set up scheduled tasks in Claude Code Desktop to run Claude automatically on a recurring basis for daily code reviews, dependency audits, or morning briefings.
- [Schedule tasks on the web](https://code.claude.com/docs/en/web-scheduled-tasks.md): Automate recurring work with cloud scheduled tasks.
