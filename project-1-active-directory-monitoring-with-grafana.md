# Project 1: Active Directory Monitoring with Grafana

## Introduction
Grafana is an open-source platform for monitoring and observability. It provides a powerful and flexible dashboard to visualize Active Directory metrics, making it easier to monitor and manage AD health and performance.

## Lab Setup and Tools
- Windows Server with Active Directory Domain Services (AD DS)
- Grafana installed on a monitoring server
- Prometheus installed as a data source for Grafana
- Prometheus Windows Exporter for collecting AD metrics

## Tool Installation

### Install Prometheus Windows Exporter
1. Download the Windows Exporter from the official Prometheus website.
2. Install the exporter on the Windows Server hosting AD DS by running the following command in PowerShell:
    ```powershell
    Start-Process -FilePath .\windows_exporter-<version>-amd64.msi -ArgumentList /quiet
    ```
3. Configure the exporter to run as a service:
    ```powershell
    New-Service -Name "wmi_exporter" -BinaryPathName "C:\Program Files\wmi_exporter\wmi_exporter.exe" -DisplayName "WMI Exporter" -StartupType Automatic
    Start-Service -Name "wmi_exporter"
    ```

### Install Prometheus
1. Download Prometheus from the official Prometheus website.
2. Extract the files and configure `prometheus.yml` to scrape metrics from the Windows Exporter by adding:
    ```yaml
    scrape_configs:
      - job_name: 'windows_exporter'
        static_configs:
          - targets: ['<AD_server_IP>:9182']
    ```
3. Start Prometheus by running:
    ```bash
    ./prometheus --config.file=prometheus.yml
    ```

### Install Grafana
1. Download [Grafana](https://grafana.com/grafana/download) from the official Grafana website.
2. Follow the installation instructions for your operating system.
3. Start Grafana by running:
    ```bash
    sudo systemctl start grafana-server
    ```
4. Enable Grafana to start on boot:
    ```bash
    sudo systemctl enable grafana-server
    ```
5. Access Grafana via `http://<your_server_ip>:3000` and log in with default credentials (`admin`/`admin`). Change the password when prompted.

## Exercises

### Exercise 1: Setting up Prometheus Data Source in Grafana
**Steps:**
1. Log in to Grafana.
2. Navigate to "Configuration" > "Data Sources".
3. Click "Add data source" and select "Prometheus".
4. Enter the Prometheus server URL (e.g., `http://<your_prometheus_server_ip>:9090`) and click "Save & Test".

**Expected Output:**
- Grafana successfully connects to Prometheus and is ready to visualize data.

### Exercise 2: Creating a Dashboard for AD Metrics
**Steps:**
1. Go to "Create" > "Dashboard".
2. Click "Add new panel".
3. Select a metric from Prometheus related to AD (e.g., `windows_logical_disk_free_bytes`).
4. Configure the visualization type (e.g., graph, gauge).
5. Click "Apply".

**Expected Output:**
- A dashboard panel displaying real-time AD metrics is created.

### Exercise 3: Monitoring AD User Logon Events
**Steps:**
1. Ensure the Windows Exporter is configured to collect Event Log metrics.
2. Create a new panel in Grafana.
3. Select a metric for user logon events (e.g., `windows_eventlog_security_logon`).
4. Apply filters to focus on specific event IDs.

**Expected Output:**
- A panel showing user logon events and their frequency is displayed.

### Exercise 4: Setting Up Alerts for AD Performance Issues
**Steps:**
1. Edit a panel showing critical AD metrics (e.g., CPU usage).
2. Go to the "Alert" tab.
3. Define alert conditions (e.g., if CPU usage > 80% for 5 minutes).
4. Configure notification channels (e.g., email, Slack).
5. Click "Save".

**Expected Output:**
- Alerts are set up to notify administrators of potential AD performance issues.

### Exercise 5: Visualizing AD Security Events
**Steps:**
1. Create a new dashboard specifically for security events.
2. Add panels for various security metrics (e.g., failed logon attempts, account lockouts).
3. Use appropriate filters to refine the data displayed.
4. Arrange panels for a comprehensive security overview.

**Expected Output:**
- A security-focused dashboard displaying key AD security metrics.

---

By following this project, you will set up a comprehensive Active Directory monitoring system using Grafana, Prometheus, and Prometheus Windows Exporter, enabling you to visualize and monitor AD health, performance, and security events.
