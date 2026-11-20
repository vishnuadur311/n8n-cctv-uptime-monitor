# n8n CCTV Network Uptime Monitor

**n8n CCTV Network Uptime Monitor** is a self-hosted workflow automation project built to monitor CCTV network endpoints and a core PoE switch on a recurring schedule and dispatch structured email alerts when devices become unreachable.

Designed and tested on **Windows 11** using **Ubuntu via WSL (Windows Subsystem for Linux)** and **Docker Desktop** container integration, the workflow runs inside **n8n** and delivers notifications through an SMTP relay. On each scheduled run, the pipeline loads target device entries (device name, device type, and IP address), probes endpoint reachability while allowing failed checks to continue downstream, and routes unreachable items through an **If** condition so healthy endpoints do not trigger unnecessary alerts.

When offline devices are detected, a custom JavaScript **Code** node (`scripts/alerts-formatter.js`) aggregates the affected endpoints, formats timestamps in `Asia/Dubai` (GST), and checks whether the core PoE switch IP is among the offline targets. If the core switch is down, the alert highlights a potential upstream power or uplink root cause affecting downstream cameras; otherwise, it notes that cameras are unreachable while the core switch remains reachable. Finally, an email node sends a clear summary to support timely field verification of PoE ports, power supplies, and cabling.

Building this as a first n8n workflow reinforced practical operational habits: distinguishing manual test executions (**Execute workflow**) from background scheduled runs (**Publish**), accounting for container and host runtime uptime during scheduled checks, and sanitizing internal IP addresses and site identifiers (`10.0.255.x` and `SITE-A`) before publishing the workflow JSON, formatter script, and workflow/alert screenshots (`cctv1.png`–`cctv3.png`) to GitHub.

## Repository Structure

* **Workflow JSON:** [`workflow/CCTV%20Network%20Uptime%20Monitor.json`](workflow/CCTV%20Network%20Uptime%20Monitor.json)
* **Alert Formatter Script:** [`scripts/alerts-formatter.js`](scripts/alerts-formatter.js)
* **Ignore Rules:** [`.gitignore`](.gitignore)

## Screenshots

### Workflow Overview
![Workflow Screenshot 1](images/cctv1.png)

### Execution & Node Configuration
![Workflow Screenshot 2](images/cctv2.png)

### Alert Output / Execution Log
![Workflow Screenshot 3](images/cctv3.png)
