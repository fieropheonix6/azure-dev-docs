---
title: How to create and configure a managed identity on Azure hosting services
description: How to create and configure a managed identity on Azure hosting services
author: KarlErickson
ms.author: v-yonghuiye
ms.service: azure-java
ms.topic: include
ms.date: 07/26/2022
---

## Create and configure a managed identity on Azure hosting services

There are two types of managed identities: system-assigned and user-assigned.

For more information about Azure managed identities, see [Azure AD-managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/).

For more information about Azure Active Directory, see [What is Azure Active Directory?](/azure/active-directory/fundamentals/active-directory-whatis)

### Create and configure a system-assigned managed identity

The following list provides references on how to create and configure system-assigned managed identity on various Azure hosting services.

- Azure VM. For more information, see [Configure managed identities for Azure resources on a VM using the Azure portal](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm).
- Azure App Service and Azure Functions. For more information, see [How to use managed identities for App Service and Azure Functions](/azure/app-service/overview-managed-identity).
- Azure Containers Instances. For more information, see [How to use managed identities with Azure Container Instances](/azure/container-instances/container-instances-managed-identity).
- Azure Kubernetes Service (AKS). For more information, see [Use a managed identity in Azure Kubernetes Service](/azure/aks/use-managed-identity).
- Azure Spring Apps. For more information, see [Enable system-assigned managed identity for an application in Azure Spring Apps](/azure/spring-cloud/how-to-enable-system-assigned-managed-identity?tabs=azure-portal&pivots=sc-standard-tier).

### Create and configure a user-assigned managed identity

For instructions on how to create a user-assigned managed identity, see [Manage user-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/how-manage-user-assigned-managed-identities?pivots=identity-mi-methods-azp).

The following list provides references on how to configure a user-assigned managed identity on various Azure hosting services.

- Azure VM. For more information, see [Configure managed identities for Azure resources on a VM using the Azure portal](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm).
- Azure App Service and Azure Functions. For more information, see [How to use managed identities for App Service and Azure Functions](/azure/app-service/overview-managed-identity).
- Azure Containers instances. For more information, see [How to use managed identities with Azure Container Instances](/azure/container-instances/container-instances-managed-identity).
- Azure Kubernetes Service(AKS). For more information, see [Use a managed identity in Azure Kubernetes Service](/azure/aks/use-managed-identity#bring-your-own-control-plane-mi).
- Azure Spring Apps. For more information, see [Manage user-assigned managed identities for an application in Azure Spring Apps](/azure/spring-cloud/how-to-manage-user-assigned-managed-identities?tabs=azure-portal&pivots=sc-standard-tier).