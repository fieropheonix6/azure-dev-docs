---
title: "[Doc Review] No verification commands between resource creation steps"
labels: documentation, doc-review, medium
---

## Issue Found in Documentation Review

**Document:** C:\Users\diberry\project-dina\repos\public-microsoftdocs-azure-dev-docs-pr\articles\github-copilot-azure\agent-mode-cosmosdb-import.md
**Severity:** Medium
**Review Date:** 2026-05-04 15:45

### Problem
No verification commands between resource creation steps

### Suggested Fix
: Add `az cosmosdb show`, `az group show`, and `az cosmosdb sql container show` verification commands after each creation step with expected output.

### Context
- Review Score: 63/100
- Step where found: General

### How to reproduce
Follow the document steps. The issue occurs when the agent attempts to execute the documented instructions.
