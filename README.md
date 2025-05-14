
# Monitor a Data Warehouse in Microsoft Fabric

This lab guides you through monitoring a data warehouse using **Microsoft Fabric**. You'll explore **Dynamic Management Views (DMVs)** and **Query Insights**. Estimated time: **30 minutes**.

> ⚠️ You must have a Microsoft Fabric trial or license enabled to complete this lab.

---

## 🚀 Steps Overview

### 1. Create a Workspace

1. Go to [Microsoft Fabric Home](https://app.fabric.microsoft.com/home?experience=fabric) and sign in.
2. Select **Workspaces** (🗇 icon).
3. Create a new workspace (name it anything) with Fabric capacity (Trial, Premium, or Fabric).
4. Your workspace should open empty.

---

### 2. Create a Sample Data Warehouse

1. On the left menu, select **Create** → under **Data Warehouse**, choose **Sample warehouse**.
2. Name it `sample-dw`.
3. Wait for the warehouse to be populated with **Taxi Ride sample data**.

---

### 3. Explore Dynamic Management Views (DMVs)

Open a **New SQL Query** and run the following:

```sql
-- View current connections
SELECT * FROM sys.dm_exec_connections;

-- View current sessions
SELECT * FROM sys.dm_exec_sessions;

-- View current requests
SELECT * FROM sys.dm_exec_requests;

-- View running queries with details
SELECT connections.connection_id,
       sessions.session_id, sessions.login_name, sessions.login_time,
       requests.command, requests.start_time, requests.total_elapsed_time
FROM sys.dm_exec_connections AS connections
JOIN sys.dm_exec_sessions AS sessions
  ON connections.session_id = sessions.session_id
JOIN sys.dm_exec_requests AS requests
  ON requests.session_id = sessions.session_id
WHERE requests.status = 'running'
  AND requests.database_id = DB_ID()
ORDER BY requests.total_elapsed_time DESC;

Simulate a long-running query:

    Open a New SQL Query tab, run:

WHILE 1 = 1
    SELECT * FROM Trip;

    Re-run the DMV query in the first tab to observe the long-running query.

    Cancel the infinite loop in the second tab and confirm it is gone from DMV results.

4. Explore Query Insights

In a New SQL Query, run:

-- View previously executed queries
SELECT * FROM queryinsights.exec_requests_history;

-- View frequently run queries
SELECT * FROM queryinsights.frequently_run_queries;

-- View long-running queries
SELECT * FROM queryinsights.long_running_queries;

🧹 Clean Up Resources

    Open your workspace.

    Go to Workspace Settings → General.

    Scroll down and select Remove this workspace → Delete.
