---
layout: default
title: Copilot Studio Agents Billing
---

# How to Set Up Billing for Copilot Studio Agents (Power Platform Admin Center)

> **Target audience:** IT Admins / Tenant Admins
> **Focus:** Step-by-step admin configuration (no pricing details)
> **Portal:** Power Platform admin center (admin.powerplatform.microsoft.com)

---

## Introduction

Copilot Studio agents built in the Power Platform use a different billing setup than Copilot Chat agents. Instead of the Microsoft 365 admin center, you configure pay-as-you-go billing from the **Power Platform admin center**.

This article walks you through the complete setup process, step by step.

---

## Prerequisites

- Power Platform Admin, Global Admin, or Dynamics 365 Admin role
- An active **Azure subscription** in your tenant (with Owner or Contributor permissions)
- A **resource group** in that Azure subscription

---

## Step 1: Navigate to Licensing > Billing Plans

In the **Power Platform admin center**, click on **Licensing** in the left navigation bar, then select **Billing Plans**.

![Screenshot 1 - Power Platform Billing Plans](screenshots/pp-01.png)

The screenshot shows the **Power Platform admin center** with the following key elements:

- **Left navigation:** The **Licensing** icon is highlighted (circled) in the sidebar. Under Licensing, **Billing Plans** is selected at the top, followed by *Capacity add-ons*. Below that, the **Products** section lists: Power Apps, Power Automate, Power Pages, Copilot Studio, Dataverse, and Finance and Operations.
- **Main content area:** The **"Billing plans"** page displays with the description: *"A pay-as-you-go plan is a group of one or more environments that you can configure to bill to Azure."*
- **Active tab:** Shows existing billing plans in a table.
- **Existing plan:** One billing plan is already present:

| Name | Status | Azure subscription name | Resource | Products | Region | Created on |
|------|--------|------------------------|----------|----------|--------|------------|
| PPCoEPlan1 | Enabled | ME-M365CPI27040012-ostefka-1 | PPCoEPlan1 | Dataverse, Power Apps, Power Auto... | United States | 10/15/2025 |

- **Toolbar:** Options include *+ New billing plan*, *Refresh list*, *See details*, *Edit*, *Download report*, *Delete billing plan*, and a *Search plans* box.

> **Note:** Unlike the M365 admin center flow, here you can see that billing plans in Power Platform can cover multiple products (Dataverse, Power Apps, Power Automate, etc.) — not just Copilot Studio.

Click **"+ New billing plan"** to create a new plan for Copilot Studio.

---

## Step 2: New Billing Plan — Select Azure Subscription

After clicking **"+ New billing plan"**, a side panel opens on the right: **"New billing plan"**.

![Screenshot 2 - New billing plan panel](screenshots/pp-02.png)

The panel asks you to **"Select an option for your new billing plan"** and shows one option:

- **Azure subscription** (circled) — *"Creating this billing plan will turn on pay-as-you-go billing for all of the resources in the environments that you select."*

Click on **Azure subscription** to proceed.

> **Note:** In the M365 admin center flow for Copilot Chat agents, you had a separate "Microsoft 365 Copilot Chat" option. Here in Power Platform, there is only the **Azure subscription** option — this is because Power Platform billing plans cover environments and their resources broadly (Copilot Studio, Dataverse, Power Apps, Power Automate, etc.).

---

## Step 3: Configure Billing Plan Details

The **New billing plan** panel expands to show the configuration form.

![Screenshot 3 - Billing plan details](screenshots/pp-03.png)

At the top, the panel explains: *"Creating this billing plan will turn on pay-as-you-go billing for all of the resources in the environments that you select. This billing plan will charge to an Azure subscription that you select."*

> **Note:** Dataverse meters will automatically be created when creating a new billing plan.

Fill in the following fields:

- **Name:** A descriptive name for your billing plan (e.g., *"MCSEnvtest"*).
- **Azure subscription:** Select the Azure subscription to bill against. You must have Owner or Contributor role on it.
- **Resource group:** Select an existing resource group (e.g., *"PP-Billing-Env-Test"*). If you don't have one, create it in the Azure portal first.
- **Meter:** Select which Power Platform products/meters to include. In this example: *"Dataverse, Windows 365 for Agents, Copilot Studio"*. This is a multi-select dropdown — you choose which product meters apply to this billing plan.

> **Key difference from M365 admin center:** Here you explicitly select which **product meters** to include in the billing plan. This gives you finer control — you can include Copilot Studio alongside Dataverse, Power Apps, Power Automate, etc., or create a dedicated plan just for Copilot Studio.

Click **Next** (circled) to proceed to the environment selection step.

---

## Step 4: Select Environments

The next step is **Select environments** — here you choose which Power Platform environments will be linked to this billing plan.

![Screenshot 4 - Select environments](screenshots/pp-04.png)

The panel explains: *"Select the environments that you'd like to add to this billing plan. Environments displayed below are filtered by the region that's selected."*

- **Region:** Select the region where your environments are located. In this lab example, *"United States"* is selected because the test environments are hosted there.

> **Important for European customers:** If your environments are provisioned in Europe, you **must select your European region** (e.g., *Europe*) here. The list only shows environments matching the selected region. If you don't see your environment, change the region dropdown first.

The table lists all available environments in the selected region (9 in this example):

| Name | Type | Dataverse | Managed | Group |
|------|------|-----------|---------|-------|
| NODV | Sandbox | No | No | None |
| PipelinesHost | Production | Yes | No | None |
| Prod | Production | Yes | Yes | None |
| SandBoxTest1 | Sandbox | Yes | No | None |
| TestCopilotAgents | Production | Yes | No | None |
| **TestRoles** | **Production** | **Yes** | **No** | **None** |

Check the box next to the environment(s) you want to add to the billing plan. In this example, **TestRoles** (a Production environment with Dataverse) is selected.

> **Tip:** You can select multiple environments at once. Pay-as-you-go is available for both **Production** and **Sandbox** environments. An environment can only be linked to one billing plan at a time.

Click **Save** (circled) to create the billing plan.

---

## Step 5: Billing Plans Overview — Configuration Complete

After saving, you're back on the **Billing plans** page. The new billing plan now appears alongside the existing one.

![Screenshot 5 - Billing plans final](screenshots/pp-05.png)

The table now shows two billing plans:

| Name | Status | Azure subscription name | Resource | Products | Region | Created on |
|------|--------|------------------------|----------|----------|--------|------------|
| MCSEnvtest | Enabled | ...fka-1 | MCSEnvtest | Dataverse, Windows 365 for Agents,... | United States | 2/15/2026 |
| PPCoEPlan1 | Enabled | ...fka-1 | PPCoEPlan1 | Dataverse, Power Apps, Power Auto... | United States | 10/15/2025 |

The newly created **MCSEnvtest** plan is now **Enabled** and linked to the Azure subscription. It covers **Dataverse, Windows 365 for Agents, and Copilot Studio** meters. The corresponding Azure resource (*MCSEnvtest*) has been automatically created in the specified resource group.

**That's it — the setup is complete!** From this point forward:
- Any Copilot Studio agent usage in the linked environment(s) will be billed to the Azure subscription via pay-as-you-go.
- You can manage the plan using the toolbar: **See details**, **Edit**, **Download report**, or **Delete billing plan**.
- Use **Download report** to export usage data for cost analysis.

---

## Summary

To enable pay-as-you-go billing for Copilot Studio agents from the **Power Platform admin center**:

1. Navigate to **Licensing > Billing Plans**
2. Click **"+ New billing plan"** and select **Azure subscription**
3. Configure the plan: name, Azure subscription, resource group, and **meter** (select Copilot Studio and any other products you need)
4. Select the **region** and **environments** to link to the billing plan
5. Click **Save**

> **Key differences from Copilot Chat agents (M365 admin center):**
> - Power Platform billing plans are **environment-based** — you select which environments to link
> - You choose which **product meters** to include (Copilot Studio, Dataverse, Power Apps, etc.)
> - There is no separate "Choose users" or "Budget" step in the wizard. Instead of assigning billing to users directly, you **assign a billing plan to environments** — and then manage which users have access to those environments. Budget management is handled through Azure Cost Management.
> - One billing plan can serve **multiple environments**, and each environment can contain **multiple products**

---

## Notes

- This flow is specifically for **Copilot Studio agents** built in Power Platform
- Everything is done from **Power Platform admin center** (admin.powerplatform.microsoft.com), NOT from M365 admin center
- The Copilot Chat agents and SharePoint agents have a different billing setup flow (covered in a separate article)
