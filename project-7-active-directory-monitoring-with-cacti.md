# Project 7: Visualizing Active Directory Performance Metrics with Cacti

## Introduction
Cacti is an open-source network monitoring and graphing tool designed as a front-end application for the data logging tool RRDtool. It is particularly useful for visualizing performance metrics from Active Directory in a comprehensive and customizable dashboard.

## Lab Setup and Tools
- Windows Server with Active Directory Domain Services (AD DS)
- Cacti installed on a monitoring server
- SNMP (Simple Network Management Protocol) enabled on AD DS servers

## Tool Installation

### Enable SNMP on AD DS Servers
1. Open Server Manager on the Windows Server.
2. Navigate to "Manage" > "Add Roles and Features".
3. Proceed through the wizard and select "SNMP Service" under "Features".
4. Complete the installation and configure SNMP:
    - Open "Services" from the Control Panel.
    - Locate and double-click "SNMP Service".
    - Go to the "Security" tab and configure community strings and allowed hosts.

### Install Cacti
1. Download Cacti from the official Cacti website.
2. Follow the installation instructions for your operating system.
3. Configure the web server (Apache, Nginx) to serve Cacti.
4. Initialize Cacti by navigating to `http://<your_server_ip>/cacti` and follow the setup wizard.

## Exercises

### Exercise 1: Configuring SNMP on AD DS Servers
**Steps:**
1. Open "Services" from the Control Panel.
2. Locate and double-click "SNMP Service".
3. Go to the "Security" tab.
4. Add a community string (e.g., "public") and configure allowed hosts (e.g., Cacti server IP).

**Expected Output:**
- SNMP is configured on AD DS servers to allow monitoring by the Cacti server.

### Exercise 2: Adding AD DS Servers to Cacti
**Steps:**
1. Log in to the Cacti web interface.
2. Navigate to "Devices" > "Add".
3. Enter the hostname and IP address of the AD DS server.
4. Select "Windows Server" as the device template.
5. Configure SNMP settings with the community string set earlier and save the device.

**Expected Output:**
- The AD DS server is added to Cacti and ready for data collection.

### Exercise 3: Creating Graphs for AD Metrics
**Steps:**
1. Go to "Create" > "New Graphs".
2. Select the AD DS server from the list of devices.
3. Choose the metrics you want to graph (e.g., CPU usage, memory usage).
4. Click "Create" to generate the graphs.

**Expected Output:**
- Graphs for various AD metrics are created and visible in the Cacti dashboard.

### Exercise 4: Monitoring AD Logon Events
**Steps:**
1. Ensure SNMP is configured to capture event logs.
2. Create a new graph template for logon events:
    - Navigate to "Graph Templates".
    - Click "Add" and define the graph for logon events.
3. Apply the graph template to the AD DS server.

**Expected Output:**
- A graph displaying logon events for the AD DS server is created.

### Exercise 5: Setting Up Alerts for AD Performance Issues
**Steps:**
1. Navigate to "Settings" > "Thresholds".
2. Create a new threshold rule for CPU usage:
    - Select the graph for CPU usage.
    - Define the threshold limits (e.g., warning at 80%, critical at 90%).
3. Configure notification settings for alerting (e.g., email).

**Expected Output:**
- Alerts are set up to notify administrators of potential AD performance issues.

### Exercise 6: Visualizing AD Security Metrics
**Steps:**
1. Create new graph templates for security metrics (e.g., failed logon attempts, account lockouts).
2. Apply these templates to the AD DS server.
3. Use appropriate SNMP OIDs to capture the data.
4. Arrange the graphs in a comprehensive dashboard.

**Expected Output:**
- A security-focused section in the Cacti dashboard displaying key AD security metrics.

---

By following this project, you will set up a comprehensive Active Directory monitoring system using Cacti, enabling you to visualize and monitor AD health, performance, and security events through detailed and customizable graphs.
