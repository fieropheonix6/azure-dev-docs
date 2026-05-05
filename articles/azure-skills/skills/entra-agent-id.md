---
title: Azure skill for Entra Agent ID
description: Provisions Microsoft Entra Agent Identity Blueprints, BlueprintPrincipals, and per-instance Agent Identities via Microsoft Graph, and configures OAuth 2.0 token exchange.
ms.topic: reference
ms.date: 05/05/2026
author: diberry
ms.author: diberry
ms.service: azure-mcp-server
ms.custom: skill-version-1.0.1
---

# Azure skill for Entra Agent ID

Provisions Microsoft Entra Agent Identity Blueprints, BlueprintPrincipals, and per-instance Agent Identities via Microsoft Graph, and configures OAuth 2.0 token exchange.

**Skill:** `entra-agent-id` | [Source code](https://github.com/microsoft/azure-skills/blob/main/skills/entra-agent-id/SKILL.md)

## What it provides

This skill provides GitHub Copilot with specialized knowledge for creating and managing OAuth 2.0-capable identities for AI agents using Microsoft Graph. Every agent instance gets a distinct identity, audit trail, and independently scoped permission grants. The skill covers the Agent Identity object model (Blueprint → BlueprintPrincipal → Agent Identity), runtime token exchange flows, and the Microsoft Entra SDK for AgentID sidecar.

## Prerequisites

- **Azure subscription**: [Create a free account](https://azure.microsoft.com/free/) if you don't have one.
- **AI assistant with Azure Skills**: [GitHub Copilot for Azure](/azure/developer/github-copilot-azure/get-started), Visual Studio Code with [Azure MCP extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azure-mcp-server), Claude Code, or another [compatible MCP client](../install.md).
- **Microsoft Entra role**: Agent Identity Developer, Agent Identity Administrator, or Application Administrator.
- **Microsoft Graph access**: PowerShell (`Microsoft.Graph.Applications`) or Python (`azure-identity`, `requests`).

## When to use this skill

Use this skill when you need to:

- Provision a new Agent Identity Blueprint and BlueprintPrincipal.
- Create per-instance Agent Identities under a Blueprint.
- Configure credentials (Federated Identity Credential, Managed Identity, or client secret) on the Blueprint.
- Implement the two-step `fmi_path` runtime token exchange (autonomous or OBO).
- Set up cross-tenant agent token flows.
- Deploy the Microsoft Entra SDK for AgentID sidecar for polyglot agents (Python, Node, Go, Java).
- Grant per-Agent-Identity application or delegated permissions.
- Diagnose Agent ID errors such as `AADSTS82001`, `AADSTS700211`, or `PropertyNotCompatibleWithAgentIdentity`.

### When not to use this skill

- Standard Entra app registration — use `entra-app-registration`.
- Azure RBAC — use `azure-rbac`.
- Microsoft Foundry agent authoring — use `microsoft-foundry`.

## Suggested workflow

The skill follows a core provisioning workflow:

1. **Create Agent Identity Blueprint**: Define the agent type/class as an application object.
1. **Create BlueprintPrincipal**: Explicitly create the service principal (not auto-created).
1. **Create Agent Identities**: Provision per-instance identities under the Blueprint.
1. **Configure credentials**: Set up authentication on the Blueprint (Workload Identity Federation for production, client secret for dev).
1. **Grant permissions**: Assign application or delegated permissions per Agent Identity.
1. **Configure runtime exchange**: Implement the two-step `fmi_path` token exchange for autonomous or OBO flows.

> [!IMPORTANT]
> `DefaultAzureCredential` is not supported for Agent Identity APIs. Azure CLI tokens carry `Directory.AccessAsUser.All`, which Agent Identity APIs reject with 403. Use a dedicated app registration with `client_credentials`, or `Connect-MgGraph` with explicit delegated scopes.

## Model Context Protocol (MCP) tools

This skill uses the following Azure MCP tools:

| Tool | Description |
|------|-------------|
| `mcp_azure_mcp_documentation` | Search Microsoft Learn for current Agent ID setup, Graph API shapes, and SDK configuration |

## Example prompts

Try these prompts to activate this skill:

- "Set up an Agent Identity Blueprint for my AI agent"
- "Create a BlueprintPrincipal for my agent"
- "Provision agent identities for my AI agents"
- "Configure OAuth for agent identity"
- "Set up fmi_path token exchange for my agent"
- "Configure agent OBO flow"
- "Set up Workload Identity Federation for agents"
- "Deploy the Microsoft Entra SDK for AgentID sidecar"
- "Configure polyglot agent authentication"

## Related content

- [Microsoft Entra Agent ID setup guide](/entra/agent-id/identity-platform/agent-id-setup-instructions)
- [Microsoft Entra SDK for AgentID](/entra/msidweb/agent-id-sdk/overview)
- [Azure Model Context Protocol (MCP) Server overview](/azure/developer/azure-mcp-server/overview)
- [Skill source code](https://github.com/microsoft/azure-skills/blob/main/skills/entra-agent-id/SKILL.md)
