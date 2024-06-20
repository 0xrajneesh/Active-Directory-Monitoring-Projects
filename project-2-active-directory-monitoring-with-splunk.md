# Project 2: Active Directory Logs and Insights with Splunk

## Introduction
Splunk is a powerful platform for searching, monitoring, and analyzing machine-generated big data. It is particularly useful for collecting and analyzing Active Directory logs to gain insights into security and operational events.

## Lab Setup and Tools
- Windows Server with Active Directory Domain Services (AD DS)
- Splunk Enterprise installed on a monitoring server
- Universal Forwarder installed on AD DS servers to collect logs

## Tool Installation

### Install Splunk Enterprise
1. Download Splunk Enterprise from the official Splunk website.
2. Follow the installation instructions for your operating system.
3. Start Splunk by running:
    ```bash
    sudo /opt/splunk/bin/splunk start --accept-license
    ```
4. Create an admin account and set the password.

### Install Universal Forwarder
1. Download Splunk Universal Forwarder from the official Splunk website.
2. Install the forwarder on all AD DS servers:
    ```bash
    msiexec.exe /i splunkforwarder-<version>-x64-release.msi /quiet
    ```
3. Configure the forwarder to send Windows Event Logs to the Splunk server:
    ```bash
    & "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" set deploy-poll <Splunk_Server_IP>:8089
    & "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" add monitor "C:\Windows\System32\winevt\Logs\Security.evtx" -sourcetype "WinEventLog:Security"
    & "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" restart
    ```

## Exercises

### Exercise 1: Configuring Data Inputs in Splunk
**Steps:**
1. Log in to Splunk.
2. Navigate to "Settings" > "Data Inputs".
3. Click "Add New" and select "Forwarded Data".
4. Configure the input to receive logs from the Universal Forwarder.

**Expected Output:**
- Splunk is configured to receive and index logs from AD DS servers.

### Exercise 2: Creating a Dashboard for AD Logon Events
**Steps:**
1. Go to "Dashboards" and click "Create New Dashboard".
2. Add a new panel and select "Search" as the data source.
3. Use a search query to filter logon events:
    ```splunk
    sourcetype="WinEventLog:Security" EventCode=4624
    ```
4. Configure visualization and save the panel.

**Expected Output:**
- A dashboard panel displaying AD logon events is created.

### Exercise 3: Analyzing AD Security Events
**Steps:**
1. Create a new search in Splunk.
2. Use a search query to filter security events (e.g., failed logon attempts, account lockouts):
    ```splunk
    sourcetype="WinEventLog:Security" (EventCode=4625 OR EventCode=4740)
    ```
3. Save the search and add it to a dashboard.
4. Configure the panel to show relevant security metrics.

**Expected Output:**
- Panels displaying critical AD security events are added to the dashboard.

### Exercise 4: Setting Up Alerts for AD Anomalies
**Steps:**
1. Create a search for an anomaly (e.g., a high number of failed logon attempts):
    ```splunk
    sourcetype="WinEventLog:Security" EventCode=4625 | stats count by User
    ```
2. Save the search and select "Alert" from the options.
3. Configure alert conditions and notification settings.
4. Save the alert.

**Expected Output:**
- Alerts are set up to notify administrators of AD anomalies.

### Exercise 5: Generating Reports on AD Activity
**Steps:**
1. Create a search for the desired AD activity report (e.g., user logon activity):
    ```splunk
    sourcetype="WinEventLog:Security" EventCode=4624 | stats count by User
    ```
2. Save the search and select "Report".
3. Configure the report schedule and format.
4. Save the report.

**Expected Output:**
- Scheduled reports on AD activity are configured and will be generated regularly.

---

By following this project, you will set up a comprehensive Active Directory monitoring system using Splunk, enabling you to collect, analyze, and visualize AD logs, and gain valuable insights into AD security and operational events.
