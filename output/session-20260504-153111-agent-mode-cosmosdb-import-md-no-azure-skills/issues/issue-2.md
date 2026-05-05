---
title: "[Doc Review] Download instruction is unnecessarily complex"
labels: documentation, doc-review, medium
---

## Issue Found in Documentation Review

**Document:** C:\Users\diberry\project-dina\repos\public-microsoftdocs-azure-dev-docs-pr\articles\github-copilot-azure\agent-mode-cosmosdb-import.md
**Severity:** Medium
**Review Date:** 2026-05-04 15:45

### Problem
Download instruction is unnecessarily complex

### Suggested Fix
: Provide a direct terminal download command as the primary method: `curl -o products.json "https://raw.githubusercontent.com/AzureCosmosDB/CosmicWorks/main/data/database-v4/product"`

### Context
- Review Score: 63/100
- Step where found: General

### How to reproduce
Follow the document steps. The issue occurs when the agent attempts to execute the documented instructions.
