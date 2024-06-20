# Project 3: Real-time Active Directory Metrics with Datadog

## Introduction
Datadog is a monitoring and analytics platform for cloud applications. It provides real-time visibility into Active Directory metrics, helping administrators ensure the health and performance of AD DS.

## Lab Setup and Tools
- Windows Server with Active Directory Domain Services (AD DS)
- Datadog Agent installed on AD DS servers

## Tool Installation

### Sign Up for Datadog
1. Create an account on the Datadog website.
2. Obtain an API key for agent installation.

### Install Datadog Agent
1. Download the Datadog Agent for Windows from the Datadog website.
2. Install the agent on all AD DS servers:
    ```powershell
    msiexec.exe /i datadog-agent-<version>.msi APIKEY=<Your_API_Key>
    ```
3. Configure the agent to collect Windows Event Logs and performance metrics by editing the `datadog.yaml` file:
    ```yaml
    logs_enabled: true

    init_config:

    instances:
      - type: eventlog
        channel_path: "System"
      - type: eventlog
        channel_path: "Security"
      - type: eventlog
        channel_path: "Application"
    ```

## Exercises

### Exercise 1: Configuring the Datadog Agent
**Steps:**
1. Log in to the Datadog web interface.
2. Navigate to "Integrations" > "Agent" > "Configuration".
3. Ensure the agent is properly installed and reporting metrics.
4. Configure the agent to collect additional AD-specific metrics if needed.

**Expected Output:**
- The Datadog Agent is configured and collecting data from AD DS servers.

### Exercise 2: Creating a Dashboard for AD Metrics
**Steps:**
1. Go to "Dashboards" and click "New Dashboard".
2. Add new widgets for key AD metrics (e.g., CPU usage, memory usage, logon events).
3. Configure each widget's visualization and data source.
4. Arrange the widgets and save the dashboard.

**Expected Output:**
- A dashboard displaying real-time AD metrics is created.

### Exercise 3: Monitoring AD Logon Events
**Steps:**
1. Ensure the Datadog Agent is collecting Windows Event Logs.
2. Create a new widget in the dashboard for logon events.
3. Use a query to filter logon events:
    ```datadog
    source:win32_event_log eventID:4624
    ```
4. Configure the widget and save the changes.

**Expected Output:**
- A widget displaying real-time logon events is added to the dashboard.

### Exercise 4: Setting Up Alerts for AD Performance Issues
**Steps:**
1. Go to "Monitors" and click "New Monitor".
2. Select the metric to monitor (e.g., CPU usage).
3. Define the alert conditions (e.g., if CPU usage > 80% for 5 minutes).
4. Configure notification channels and save the monitor.

**Expected Output:**
- Alerts are set up to notify administrators of potential AD performance issues.

### Exercise 5: Visualizing AD Security Metrics
**Steps:**
1. Create new widgets in the dashboard for security metrics (e.g., failed logon attempts, account lockouts).
2. Use appropriate queries to filter the data.
3. Configure the visualization for each widget.
4. Arrange the widgets to create a comprehensive security overview.

**Expected Output:**
- A security-focused section in the dashboard displaying key AD security metrics.

---

By following this project, you will set up a comprehensive Active Directory monitoring system using Datadog, enabling you to visualize and monitor AD health, performance, and security events in real-time.
