# Setup Guide

> ⚠️ **Work In Progress**
> 
> This setup guide is currently under development and **not yet tested** in a production environment.
> Use with caution. Feedback is welcome!


## 🧪 Setup Instructions

### 1. **Set up Azure Monitor and Log Analytics**

Azure Monitor is used to collect, analyze, and act on telemetry from your cloud environment. This step involves configuring Azure VMs to send activity logs to the **Log Analytics Workspace**.

#### Step 1: Create a Log Analytics Workspace

1. Go to the [Azure Portal](https://portal.azure.com/).
2. In the **Search** bar, type **Log Analytics** and select **Log Analytics Workspaces**.
3. Click on **+ Add** to create a new workspace.
4. Fill in the necessary details:
   - **Subscription**: Choose your subscription.
   - **Resource Group**: Select an existing resource group or create a new one.
   - **Name**: Choose a unique name for your workspace (e.g., `MyVMLogs`).
   - **Region**: Select the region where the logs will be stored.
5. Once you have entered the details, click **Review + Create**, then click **Create**.

#### Step 2: Configure Azure Virtual Machines to Send Logs

1. In the **Azure Portal**, navigate to your **Virtual Machine**.
2. Under **Monitoring**, select **Diagnostics settings**.
3. Click **+ Add diagnostic setting**.
4. Choose the log categories you want to send to the Log Analytics Workspace (e.g., **Guest OS** and **Boot diagnostics**).
5. Under **Destination details**, select **Send to Log Analytics** and choose the workspace you created earlier.
6. Click **Save**.

### 2. **Create and Configure the Azure Monitor Alert**

Azure Monitor will use a **Log-based Alert** to detect reboots triggered by automation. The alert will trigger an **Azure Function** when the condition is met.

#### Step 1: Create a Log-based Alert

1. In the **Azure Portal**, go to **Azure Monitor** (you can search for it in the search bar).
2. On the **Monitor** page, select **Alerts** from the left-hand sidebar.
3. Click **+ New alert rule**.
4. Under **Scope**, click **Select resource** and choose the **Log Analytics Workspace** you created.
5. Under **Condition**, click **Add condition** and select **Log query**.
6. Paste the following KQL query to detect automated reboots:
   ```kql
   Event
   | where EventID == 1074 and (Message contains "Automated")
