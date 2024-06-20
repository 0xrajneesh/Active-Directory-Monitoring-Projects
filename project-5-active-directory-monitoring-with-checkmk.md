# Project 5: Active Directory Monitoring with Checkmk

## Introduction
Checkmk is a comprehensive IT monitoring tool that provides real-time insights into your IT infrastructure. It includes a robust dashboard for monitoring Active Directory performance, health, and security events.

## Lab Setup and Tools
- Windows Server with Active Directory Domain Services (AD DS)
- Checkmk Raw Edition installed on a monitoring server
- Checkmk Agent installed on AD DS servers for monitoring

## Tool Installation

### Install Checkmk Raw Edition
1. Download Checkmk Raw Edition from the official Checkmk website.
2. Follow the installation instructions for your operating system.
3. Start Checkmk by running:
    ```bash
    sudo omd create mysite
    sudo omd start mysite
    ```
4. Access Checkmk via `http://<your_server_ip>/mysite` and log in with the default credentials (`omdadmin`).

### Install Checkmk Agent
1. Download the Checkmk Agent for Windows from the Checkmk website.
2. Install the agent on all AD DS servers:
    ```powershell
    msiexec.exe /i check-mk-agent-<version>-x64.msi /quiet
    ```

## Exercises

### Exercise 1: Adding AD DS Servers to Checkmk
**Steps:**
1. Log in to the Checkmk web interface.
2. Navigate to "Hosts" > "Add Host".
3. Enter the hostname and IP address of the AD DS server.
4. Assign the appropriate tags and click "Save & Go to Services".
5. Click "Fix all" to apply the changes.

**Expected Output:**
- The AD DS server is added to Checkmk and its services are being monitored.

### Exercise 2: Monitoring AD Logon Events
**Steps:**
1. Ensure the Checkmk Agent is configured to collect Windows Event Logs.
2. Navigate to "WATO" > "Host & Service Parameters".
3. Create a new rule in "Logwatch Event Console" to filter and monitor logon events:
    ```cfg
    logfile:
      - 'Application'
      - 'Security'
      - 'System'
    entries:
      - '4624'
    ```
4. Apply the rule and activate the changes.

**Expected Output:**
- Checkmk displays logon events for the AD DS server.

### Exercise 3: Setting Up Alerts for AD Performance Issues
**Steps:**
1. Navigate to "WATO" > "Host & Service Parameters".
2. Create a new rule in "Monitoring Configuration" to monitor CPU usage:
    ```cfg
    conditions:
      - 'CPU utilization'
    thresholds:
      - 'Warning if above 80%'
      - 'Critical if above 90%'
    ```
3. Apply the rule and activate the changes.

**Expected Output:**
- Alerts are set up to notify administrators of potential AD performance issues.

### Exercise 4: Creating a Dashboard for AD Metrics
**Steps:**
1. Go to "Views" > "Add View".
2. Add new views for key AD metrics (e.g., CPU usage, memory usage, logon events).
3. Configure each view's layout and data source.
4. Arrange the views and save the dashboard.

**Expected Output:**
- A dashboard displaying real-time AD metrics is created.

### Exercise 5: Visualizing AD Security Metrics
**Steps:**
1. Configure the Checkmk Agent to collect additional security metrics if needed.
2. Navigate to "WATO" > "Host & Service Parameters".
3. Create new rules in "Logwatch Event Console" to monitor security events (e.g., failed logon attempts, account lockouts):
    ```cfg
    logfile:
      - 'Security'
    entries:
      - '4625'
      - '4740'
    ```
4. Apply the rules and activate the changes.

**Expected Output:**
- Views in Checkmk display key AD security metrics, providing a comprehensive security overview.

---

By following this project, you will set up a comprehensive Active Directory monitoring system using Checkmk, enabling you to monitor the health, performance, and security events of AD DS servers.
