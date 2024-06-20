# Project 6: Active Directory Monitoring and Alerting with Prometheus

## Introduction
Prometheus is an open-source monitoring and alerting toolkit that provides powerful data collection and querying capabilities. It can be used to monitor Active Directory metrics in real-time and set up alerts for various performance and security events.

## Lab Setup and Tools
- Windows Server with Active Directory Domain Services (AD DS)
- Prometheus installed on a monitoring server
- Prometheus Windows Exporter for collecting AD metrics
- Alertmanager for handling alerts from Prometheus

## Tool Installation

### Install Prometheus Windows Exporter
1. Download the Windows Exporter from the official Prometheus website.
2. Install the exporter on the Windows Server hosting AD DS by running the following command in PowerShell:
    ```powershell
    Start-Process -FilePath .\windows_exporter-<version>-amd64.msi -ArgumentList /quiet
    ```
3. Configure the exporter to run as a service:
    ```powershell
    New-Service -Name "windows_exporter" -BinaryPathName "C:\Program Files\windows_exporter\windows_exporter.exe" -DisplayName "Windows Exporter" -StartupType Automatic
    Start-Service -Name "windows_exporter"
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

### Install Alertmanager
1. Download Alertmanager from the official Prometheus website.
2. Extract the files and configure `alertmanager.yml` to handle alerts:
    ```yaml
    global:
      resolve_timeout: 5m
    route:
      receiver: 'email-alert'
    receivers:
      - name: 'email-alert'
        email_configs:
          - to: 'admin@example.com'
            from: 'alertmanager@example.com'
            smarthost: 'smtp.example.com:587'
            auth_username: 'alertmanager@example.com'
            auth_password: 'password'
    ```
3. Start Alertmanager by running:
    ```bash
    ./alertmanager --config.file=alertmanager.yml
    ```

## Exercises

### Exercise 1: Configuring Prometheus Data Source
**Steps:**
1. Open the `prometheus.yml` file.
2. Ensure the configuration to scrape metrics from the Windows Exporter is correct.
3. Add Alertmanager configuration to `prometheus.yml`:
    ```yaml
    alerting:
      alertmanagers:
        - static_configs:
            - targets:
              - 'localhost:9093'
    rule_files:
      - "alert.rules"
    ```
4. Restart Prometheus:
    ```bash
    ./prometheus --config.file=prometheus.yml
    ```

**Expected Output:**
- Prometheus is configured to scrape data from the Windows Exporter and send alerts to Alertmanager.

### Exercise 2: Creating a Dashboard for AD Metrics
**Steps:**
1. Access the Prometheus web interface at `http://<your_prometheus_server_ip>:9090`.
2. Go to "Status" > "Targets" to verify the Windows Exporter is being scraped.
3. Create a new Grafana dashboard:
   - Log in to Grafana.
   - Navigate to "Configuration" > "Data Sources".
   - Add Prometheus as a data source.
   - Create a new dashboard and add panels to visualize key AD metrics (e.g., CPU usage, memory usage, logon events).

**Expected Output:**
- A Grafana dashboard displaying real-time AD metrics is created.

### Exercise 3: Monitoring AD Logon Events
**Steps:**
1. Ensure the Windows Exporter is configured to collect Event Log metrics.
2. Create a new Prometheus alert rule for logon events:
    ```yaml
    groups:
      - name: AD_Logon_Events
        rules:
          - alert: HighLogonEvents
            expr: increase(windows_eventlog_security{EventID="4624"}[5m]) > 10
            for: 10m
            labels:
              severity: warning
            annotations:
              summary: "High number of logon events"
              description: "More than 10 logon events in the last 5 minutes."
    ```
3. Add the alert rule file to `prometheus.yml` under `rule_files`:
    ```yaml
    rule_files:
      - "alert.rules"
    ```

**Expected Output:**
- Prometheus monitors logon events and triggers alerts for high activity.

### Exercise 4: Setting Up Alerts for AD Performance Issues
**Steps:**
1. Create a new Prometheus alert rule for CPU usage:
    ```yaml
    groups:
      - name: AD_Performance
        rules:
          - alert: HighCPUUsage
            expr: windows_cpu_time{mode="user"} > 80
            for: 5m
            labels:
              severity: critical
            annotations:
              summary: "High CPU Usage"
              description: "CPU usage is above 80% for more than 5 minutes."
    ```
2. Add the alert rule file to `prometheus.yml` under `rule_files`.
3. Restart Prometheus to apply the changes:
    ```bash
    ./prometheus --config.file=prometheus.yml
    ```

**Expected Output:**
- Prometheus alerts administrators when CPU usage exceeds the threshold.

### Exercise 5: Visualizing AD Security Metrics
**Steps:**
1. Ensure the Windows Exporter is collecting security metrics.
2. Create new Grafana panels for security metrics (e.g., failed logon attempts, account lockouts).
3. Use appropriate Prometheus queries to filter the data:
    ```prometheus
    windows_eventlog_security{EventID="4625"}
    windows_eventlog_security{EventID="4740"}
    ```
4. Configure the visualization for each panel and arrange them on the dashboard.

**Expected Output:**
- A security-focused section in the Grafana dashboard displaying key AD security metrics.

---

By following this project, you will set up a comprehensive Active Directory monitoring system using Prometheus, Alertmanager, and Grafana, enabling you to visualize and monitor AD health, performance, and security events in real-time.
