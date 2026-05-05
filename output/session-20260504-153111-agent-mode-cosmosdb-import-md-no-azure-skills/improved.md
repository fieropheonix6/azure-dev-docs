---
title: "Quickstart: Import data into Azure Cosmos DB with Copilot agent mode"
description: Learn how to use GitHub Copilot for Azure agent mode to create Azure Cosmos DB resources and import sample data using Python prompts in Visual Studio Code.
author: bobtabor-msft
ms.author: rotabor
keywords: github, copilot, ai, azure, cosmos db
ms.service: github-copilot-for-azure
ms.topic: quickstart
ms.date: 05/04/2026
ms.collection: ce-skilling-ai-copilot
ms.custom: msecd-doc-authoring-108
ai-usage: ai-generated

#customer intent: As a developer, I want to use GitHub Copilot agent mode to create Azure Cosmos DB resources and import data so that I can quickly set up a database without writing infrastructure code manually.
---

# Quickstart: Import data into Azure Cosmos DB using GitHub Copilot for Azure agent mode

In this quickstart, you use GitHub Copilot for Azure agent mode to create Azure Cosmos DB for NoSQL resources and import sample product data using Python. Agent mode takes action in Visual Studio Code (VS Code) — creating files, executing terminal commands, and managing dependencies on your behalf.

## Quick start summary

| Task | Method | Estimated time |
|------|--------|----------------|
| Set up environment | Manual + agent mode | 5 minutes |
| Create Azure resources | Agent mode prompts | 5–8 minutes |
| Import data | Agent mode generates Python | 2–3 minutes |
| Verify and clean up | Agent mode or CLI | 2 minutes |
| **Total** | | **15–20 minutes** |

## Prerequisites

| Prerequisite | Install | Verify |
|---|---|---|
| Azure subscription | [Create free account](https://azure.microsoft.com/free/) | `az account show` |
| Visual Studio Code | [Download VS Code](https://code.visualstudio.com/) | `code --version` |
| GitHub Copilot for Azure extension | [Install from Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azure-github-copilot) | Check **Extensions** panel in VS Code |
| Python 3.9 or later | [Download Python](https://www.python.org/downloads/) | `python --version` |
| Azure CLI 2.60 or later | [Install Azure CLI](/cli/azure/install-azure-cli) | `az --version` |
| GitHub Copilot subscription | [GitHub Copilot plans](https://github.com/features/copilot) | Agent mode available in Copilot Chat |

You also need:

- Familiarity with [getting started with GitHub Copilot for Azure](get-started.md).
- Sufficient Azure role-based access control (RBAC) permissions to create resource groups and Azure Cosmos DB resources (Contributor role or higher on the subscription).

## Set up the environment

### Create a workspace directory

1. Create a new directory on your local computer for this project.
1. Open VS Code from that directory. You can run `code .` in a terminal from inside the directory.

**Verify**: The VS Code Explorer panel shows your empty directory as the workspace root.

### Configure agent mode

1. In VS Code, open the GitHub Copilot Chat panel by selecting **View** > **Chat**.
1. At the top of the chat panel, select the mode dropdown and choose **Agent**.
1. Select your preferred language model (for example, GPT-4o or Claude Sonnet).

**Verify**: The chat panel shows "Agent" mode in the mode selector and displays available tools.

### Download the sample data

Download the CosmicWorks product data file directly using one of these methods:

**Option A: Use a terminal command**

# [PowerShell](#tab/powershell)

```powershell
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/AzureCosmosDB/CosmicWorks/main/data/database-v4/product" -OutFile "products.json"
```

# [Bash / macOS / Linux](#tab/bash)

```bash
curl -o products.json "https://raw.githubusercontent.com/AzureCosmosDB/CosmicWorks/main/data/database-v4/product"
```

---

**Option B: Download from GitHub**

1. Navigate to the [CosmicWorks product data](https://github.com/AzureCosmosDB/CosmicWorks/blob/main/data/database-v4/product) page.
1. Select the **Download raw file** icon (download arrow) to save the file.
1. Move the downloaded file into your workspace directory.
1. Rename the file to `products.json`.

**Verify**: Run the following command to confirm the file contains product data:

# [PowerShell](#tab/powershell)

```powershell
(Get-Content products.json | ConvertFrom-Json).Count
```

# [Bash / macOS / Linux](#tab/bash)

```bash
python -c "import json; print(len(json.load(open('products.json'))))"
```

---

Expected output: `295` (the number of product items in the dataset).

## Create a Python virtual environment

Ask GitHub Copilot agent mode to create the environment:

```text
Create a Python virtual environment in this workspace
```

Agent mode creates the `.venv` directory and activates it. Alternatively, create the environment yourself using a terminal:

# [PowerShell](#tab/powershell)

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

# [Git Bash](#tab/bash)

```bash
python -m venv .venv
source .venv/Scripts/activate
```

# [macOS/Linux](#tab/linux)

```bash
python -m venv .venv
source .venv/bin/activate
```

---

**Verify**: Your terminal prompt shows `(.venv)` as a prefix, confirming the virtual environment is active. Run:

```bash
python --version
```

Expected output: Python 3.9 or later (for example, `Python 3.12.4`).

## Plan the work

Breaking work into smaller prompts produces better results than a single large request. Structure your prompts around these two high-level tasks:

1. **Create Azure resources** — resource group, Azure Cosmos DB account, database, and container.
1. **Import data** — generate and run Python code to upload JSON records into the container.

Each of the following sections provides the specific prompt to give agent mode. Let agent mode control the execution — select **Continue** or **Always allow** when it asks to run terminal commands.

:::image type="content" source="media/agent-mode-cosmosdb-import/github-copilot-continue-always-allow.png" alt-text="Screenshot that shows response to a prompt with an option highlighted to always allow.":::

## Build the solution

Use the following prompts in sequence. Replace all `<placeholder>` values with your own names before submitting each prompt.

| Placeholder | Description | Example |
|---|---|---|
| `<resource-group-name>` | Name for your Azure resource group | `rg-cosmosdb-quickstart` |
| `<cosmos-db-account-name>` | Globally unique Cosmos DB account name | `cosmos-myapp-dev` |
| `<database-name>` | Name for your database | `cosmicworks` |
| `<container-name>` | Name for your container | `products` |

### Sign in to Azure

If you work with multiple tenants and subscriptions, confirm you're signed in to the correct one. Use this prompt:

```text
How do I sign in to my Azure tenant and choose an Azure subscription to work with?
```

Agent mode responds with steps similar to:

1. Open a terminal.
1. Sign in to Azure: `az login`
1. List available subscriptions: `az account list --output table`
1. Set the active subscription: `az account set --subscription "<your-subscription-id>"`

Follow these steps. To confirm your active context at any time, use this prompt:

```text
Which tenant and subscription am I working with?
```

**Verify**: Run the following command in your terminal:

```azurecli
az account show --query "{subscription:name, tenantId:tenantId}" --output table
```

Expected output: A table showing your subscription name and tenant ID.

### Create a resource group

Use this prompt:

```text
Use the Azure CLI to create a resource group named `<resource-group-name>` in the `westus` region
```

Replace `<resource-group-name>` with a unique name for your resource group.

**Verify**: Run in your terminal:

```azurecli
az group show --name <resource-group-name> --query "{name:name, location:location, state:properties.provisioningState}" --output table
```

Expected output:

```
Name                      Location    State
------------------------  ----------  ---------
<resource-group-name>     westus      Succeeded
```

### Create an Azure Cosmos DB account

Use this prompt:

```text
Use the Azure CLI to create a new Azure Cosmos DB for NoSQL account named `<cosmos-db-account-name>` in the resource group `<resource-group-name>`
```

> [!NOTE]
> This step takes 3–5 minutes. Azure Cosmos DB account creation involves global DNS registration and resource provisioning.

**Verify**: Run in your terminal:

```azurecli
az cosmosdb show --name <cosmos-db-account-name> --resource-group <resource-group-name> --query "{name:name, status:provisioningState, endpoint:documentEndpoint}" --output table
```

Expected output:

```
Name                      Status     Endpoint
------------------------  ---------  ------------------------------------------------
<cosmos-db-account-name>  Succeeded  https://<cosmos-db-account-name>.documents.azure.com:443/
```

### Create a database

Use this prompt:

```text
Use the Azure CLI to create a new database named `<database-name>` in the Azure Cosmos DB account `<cosmos-db-account-name>`
```

**Verify**: Run in your terminal:

```azurecli
az cosmosdb sql database show --account-name <cosmos-db-account-name> --resource-group <resource-group-name> --name <database-name> --query "{name:name}" --output table
```

Expected output:

```
Name
--------------
<database-name>
```

### Create a container

Use this prompt:

```text
Use the Azure CLI to create a new container named `<container-name>` with partition key `/id` in the `<database-name>` database of the `<cosmos-db-account-name>` account
```

**Verify**: Run in your terminal:

```azurecli
az cosmosdb sql container show --account-name <cosmos-db-account-name> --resource-group <resource-group-name> --database-name <database-name> --name <container-name> --query "{name:name, partitionKey:resource.partitionKey.paths[0]}" --output table
```

Expected output:

```
Name              PartitionKey
----------------  -------------
<container-name>  /id
```

### Generate and run the Python import script

Use this prompt:

```text
Generate Python code to insert all JSON records from the `products.json` file into the `<container-name>` container in my Azure Cosmos DB database `<database-name>`. Use the azure-cosmos package. Use upsert_item for each record. Print progress every 50 items.
```

Agent mode:

1. Creates a Python script (for example, `upload_data.py`).
1. Installs the `azure-cosmos` package in your virtual environment.
1. Runs the script.

Review the generated code before allowing execution. The script should resemble:

```python
import json
from azure.cosmos import CosmosClient

# Connection details (agent mode retrieves these from your Azure environment)
ENDPOINT = "https://<cosmos-db-account-name>.documents.azure.com:443/"
KEY = "<your-primary-key>"
DATABASE_NAME = "<database-name>"
CONTAINER_NAME = "<container-name>"

client = CosmosClient(ENDPOINT, KEY)
database = client.get_database_client(DATABASE_NAME)
container = database.get_container_client(CONTAINER_NAME)

with open("products.json", "r") as f:
    products = json.load(f)

print(f"Uploading {len(products)} items...")

for i, item in enumerate(products):
    container.upsert_item(item)
    if (i + 1) % 50 == 0:
        print(f"  Uploaded {i + 1}/{len(products)} items")

print(f"Upload complete: {len(products)} items imported.")
```

**Verify**: The script output shows all 295 items uploaded without errors:

```
Uploading 295 items...
  Uploaded 50/295 items
  Uploaded 100/295 items
  Uploaded 150/295 items
  Uploaded 200/295 items
  Uploaded 250/295 items
Upload complete: 295 items imported.
```

## Verify the import

After the script runs, confirm the data exists in your container. Use this prompt:

```text
Query the first 5 items from the `<container-name>` container in my Cosmos DB database and show their names and prices.
```

Alternatively, add a verification script. Ask agent mode:

```text
Generate a Python script that queries the first 5 items from the `<container-name>` container and prints their id, name, and price fields.
```

**Verify**: The output shows product data, for example:

```
Query returned 5 items:
  - LL Road Seat/Saddle (price: $27.12)
  - Touring-1000 Blue, 50 (price: $2384.07)
  - ML Road Pedal (price: $62.09)
  - Mountain Bottle Cage (price: $9.99)
  - HL Road Tire (price: $32.60)
```

You can also verify the total document count using the Azure CLI:

```azurecli
az cosmosdb sql container show --account-name <cosmos-db-account-name> --resource-group <resource-group-name> --database-name <database-name> --name <container-name> --query "resource.statistics[0].documentCount" --output tsv
```

Expected output: `295`

## Clean up resources

When you're finished, delete the resource group to avoid ongoing charges.

Use this prompt in agent mode:

```text
Use the Azure CLI to delete the resource group `<resource-group-name>` and all its resources without prompting for confirmation.
```

Or run the command directly in your terminal:

```azurecli
az group delete --name <resource-group-name> --yes --no-wait
```

**Verify**: Confirm the resource group is being deleted:

```azurecli
az group show --name <resource-group-name> --query "properties.provisioningState" --output tsv 2>&1
```

Expected output: `Deleting` or `(ResourceGroupNotFound) Resource group '<resource-group-name>' could not be found.`

## Troubleshooting

| Issue | Cause | Resolution |
|---|---|---|
| `az login` opens a browser but doesn't complete | Browser-based authentication timeout | Run `az login --use-device-code` for device code flow instead |
| Cosmos DB account creation fails with naming error | Account names must be globally unique, lowercase, 3-44 characters | Choose a different name with a unique suffix (for example, your initials + date) |
| Python script fails with `ModuleNotFoundError: No module named 'azure.cosmos'` | Package not installed in the active virtual environment | Run `pip install azure-cosmos` in your activated `.venv` |
| Script fails with `CosmosHttpResponseError: (Unauthorized)` | Azure CLI credentials expired or wrong subscription selected | Run `az login` again, then verify with `az account show` |
| Script fails with `partition key mismatch` | Container partition key doesn't match data field | Ensure you created the container with `/id` as the partition key path |
| Agent mode doesn't execute commands | VS Code trust settings or agent mode permissions | Select **Always allow** when prompted, or check VS Code workspace trust settings |
| `products.json` download returns HTML instead of JSON | URL copied from GitHub web view (not raw content) | Use the raw URL: `https://raw.githubusercontent.com/AzureCosmosDB/CosmicWorks/main/data/database-v4/product` |
| Script runs but uploads 0 items | Empty or malformed `products.json` | Verify file content: `python -c "import json; print(len(json.load(open('products.json'))))"` should return `295` |

## Related content

- [Get started with GitHub Copilot for Azure](get-started.md)
- [Deploy an app with agent mode](quickstart-deploy-app-agent-mode.md)
- [Azure Cosmos DB for NoSQL documentation](/azure/cosmos-db/nosql/)
- [Azure Cosmos DB Python SDK reference](/python/api/azure-cosmos/azure.cosmos)