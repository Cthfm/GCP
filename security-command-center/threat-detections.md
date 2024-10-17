# Threat Detections

#### **Threat Detection Overview in SCC**

SCC provides **native detectors** and supports **custom detectors** to identify a variety of threats. With **SCC Premium**, organizations benefit from **real-time monitoring** and **threat intelligence** for faster incident response.

**Key Threat Detection Features:**

1. **Malware Detection**: Detects malware in virtual machines and containers.
2. **Crypto-mining Detection**: Identifies unauthorized mining workloads running in your environment.
3. **Public Exposure Alerts**: Detects when sensitive resources (e.g., Storage Buckets) become publicly accessible.
4. **IAM Anomalies**: Identifies unusual changes to IAM roles or permissions.

***

#### **4.2 Google-Provided Detectors in SCC Premium**

Google Cloud provides several **built-in detectors** for SCC, offering automated threat detection across services.

**Built-in Detectors:**

1. **Event Threat Detection (ETD):**
   * Detects **suspicious behavior** in real time, such as compromised credentials or malicious network activity.
   * **Example:** Detects brute force attacks against GCP services.
2. **Security Health Analytics (SHA):**
   * Identifies **misconfigurations** and policy violations that can increase security risks.
   * **Example:** Alerts you if a **storage bucket** is set to public access.
3. **Container Threat Detection (CTD):**
   * Monitors for **runtime security issues** in Kubernetes clusters.
   * **Example:** Detects containers with known vulnerabilities running in Google Kubernetes Engine (GKE).
4. **OS Vulnerability Detection:**
   * Scans VMs for **outdated operating systems** and packages that could be exploited by attackers.

***

#### **4.3 Creating Custom Detectors**

With **SCC Premium**, organizations can build **custom detectors** to detect specific threats based on their environment and business needs.

**Steps to Create a Custom Detector:**

1. **Identify the Use Case:**
   * **Example:** Detect any firewall rule that opens SSH (port 22) to the internet.
2. **Define the Detection Logic:**
   * Use **regular expressions** to match patterns or conditions.
3. **Configure the Detector in SCC:**
   * Go to **SCC Settings > Custom Detectors** and create a new detector.
4. **Set Severity Levels:**
   * Define whether the issue is **low, medium, high, or critical**.
5. **Test the Detector:**
   * Create a mock firewall rule change to ensure the detector triggers properly.

***

#### **4.4 Threat Intelligence and SCC Integration**

**Threat intelligence** integration enriches SCC findings by correlating events with known threat data.

* **Integration with Chronicle:**
  * Use Chronicle’s **threat feeds** to add context to findings.
  * **Example:** SCC alerts you about suspicious network activity; Chronicle correlates it with a known **malware IP**.
* **Third-Party Security Tools:**
  * SCC findings can be **exported to Splunk or Chronicle SIEM** for further analysis.

***

#### **4.5 Real-Time Alerting with SCC**

**SCC Alerts** notify teams of potential threats immediately, helping them respond before incidents escalate.

**How to Set Up Real-Time Alerts:**

1. **Enable Pub/Sub Integration:**
   * Go to **SCC Settings > Alerting** and create a **Pub/Sub topic** for alerts.
2. **Create Alert Rules:**
   * Define alert rules for specific findings (e.g., “Alert if VM is running crypto-mining software”).
3. **Configure Alert Channels:**
   * Send alerts to **Slack, Email, or PagerDuty** for fast response.
4. **Automate Incident Response:**
   * Use **Cloud Functions** to trigger remediation (e.g., shutdown a VM) when an alert triggers.

***

#### **4.6 Example: Automating Responses to Crypto-mining Detection**

**Scenario:** Your security team wants to automatically **quarantine VMs** running crypto-mining software.

**Solution Workflow:**

1. **Create a Pub/Sub Alert:**
   * Configure SCC to send a **Pub/Sub message** whenever crypto-mining is detected.
2. **Deploy a Cloud Function:**
   * Write a **Cloud Function** to stop the affected VM.
3. **Link the Cloud Function to the Alert:**
   * Set the **Pub/Sub topic** to trigger the Cloud Function automatically.

```python
pythonCopy codefrom google.cloud import compute_v1

def stop_vm(event, context):
    project_id = 'your-project-id'
    zone = 'us-central1-a'
    instance_name = 'malicious-vm'

    compute = compute_v1.InstancesClient()
    compute.stop(project=project_id, zone=zone, instance=instance_name)
    print(f"VM {instance_name} has been stopped.")
```

***

#### **4.7 SCC Threat Hunting with BigQuery**

SCC findings can be **exported to BigQuery** for deeper analysis. This enables security teams to **correlate findings, analyze trends, and build dashboards** for reporting.

**Example Query: Identify IAM Privilege Escalation Attempts**

```sql
sqlCopy codeSELECT
  finding_id,
  resource_name,
  severity,
  timestamp
FROM
  `your-project-id.scc_findings_dataset.scc_findings_table`
WHERE
  category = 'IAM Privilege Escalation'
  AND severity = 'HIGH'
  AND timestamp > TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 7 DAY)
ORDER BY
  timestamp DESC;
```

***

#### **4.8 Hands-On Lab: Detecting and Responding to Threats with SCC Premium**

In this hands-on lab, participants will:

1. **Enable Event Threat Detection (ETD).**
2. **Create a Custom Detector** to alert on unauthorized firewall changes.
3. **Set Up Pub/Sub Alerts** for crypto-mining detection.
4. **Write a Cloud Function** to automatically shut down affected VMs.
5. **Export SCC Findings to BigQuery** and query them using SQL.

***

#### **4.9 Best Practices for Threat Detection and Monitoring**

1. **Enable Native Detectors:** Always enable **Event Threat Detection** and **Security Health Analytics** to identify common threats.
2. **Create Custom Detectors:** Build detectors specific to your business needs, such as tracking changes to sensitive resources.
3. **Automate Response Workflows:** Use **Pub/Sub and Cloud Functions** to reduce manual intervention and ensure fast mitigation.
4. **Export Findings to BigQuery:** Use **BigQuery** to analyze trends and identify patterns of suspicious activity over time.
5. **Review and Tune Detectors Regularly:** Periodically **review SCC findings** and **update detectors** to match evolving security threats.
