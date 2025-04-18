# 🧪 Setup Guide (Work in Progress)

> ⚠️ **Disclaimer**  
> This guide is currently in development and **not yet tested in a production environment**.  
> It is part of a DevOps learning project and subject to changes and improvements.

---

## 📦 Prerequisites

Before getting started, make sure you have:

- An active **Azure subscription**
- At least one **Azure Virtual Machine** deployed
- **Log Analytics Workspace** and **Azure Monitor** configured
- Access to **Azure Functions** in your subscription
- Optional: **SendGrid**, **Microsoft Teams Webhook**, or **Slack Webhook** for notifications

---

## 🧱 Step-by-Step Setup

### 1. Create a Log Analytics Workspace

- Go to the [Azure Portal](https://portal.azure.com)
- Search for **Log Analytics Workspace**
- Click **Create**, then fill in:
  - Subscription
  - Resource Group
  - Workspace Name
  - Region
- Click **Review + Create** → **Create**

---

### 2. Connect Your VM to the Workspace

- Go to your **Virtual Machine**
- Under **Monitoring**, select **Insights**
- Click **Enable** and connect it to your Log Analytics Workspace

---

### 3. Set Up Azure Monitor Alert (Log Query)

- Go to **Azure Monitor**
- Choose **Alerts** > **New Alert Rule**
- Set:
  - **Scope**: Select your Log Analytics Workspace
  - **Condition**: Use a Kusto Query Language (KQL) query to detect reboot events from automation
  - **Action**: Choose **Action Group** → Add a webhook or trigger an Azure Function
  - **Alert Details**: Name your alert rule

> Example KQL Query (customize based on your logs):
```kusto
Event
| where EventLevelName == "Information"
| where Source == "Service Control Manager"
| where RenderedDescription has "restart" or "reboot"
