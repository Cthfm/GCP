# Core Components

## **Overview of SCC Components**

SCC organizes its operations around three primary components:

1. **Asset Inventory** – Track all Google Cloud resources in real time.
2. **Findings** – Detect threats, vulnerabilities, and misconfigurations.
3. **Sources & Detectors** – Collect and analyze security events from Google-native and custom sources.

Each component contributes to a unified security management experience, helping organizations stay on top of their security posture.

### **Asset Inventory**

The **Asset Inventory** provides visibility into all resources across Google Cloud projects. This feature ensures you have continuous **real-time insight** into virtual machines, storage buckets, databases, firewall rules, and more.

**Capabilities of Asset Inventory:**

* **Continuous Discovery:** Automatically detects resources in your GCP projects.
* **Asset Metadata Tracking:** Collects metadata like permissions, network configurations, and locations.
* **Drift Detection:** Detects changes in cloud resources over time (e.g., modified IAM roles, deleted firewalls).
* **Asset Filters:** Filter assets by resource type, name, or project to narrow your search.

**Example Use Case:**

You need to track **publicly accessible resources** like storage buckets and virtual machines with exposed IPs. With Asset Inventory, you can filter and tag such resources to monitor or restrict access as needed.

### **Findings**

**Findings** are at the heart of SCC’s detection capabilities. They represent security-related issues, such as **misconfigurations**, **potential attacks**, or **compliance violations**.

**Types of Findings:**

1. **Misconfiguration Findings** – Issues with resource setup (e.g., public buckets or missing firewall rules).
2. **IAM Policy Issues** – Detects risky permissions, such as over-permissive roles.
3. **Threat Findings** – Identifies malicious activities (e.g., malware or crypto-mining).
4. **Compliance Violations** – Highlights non-compliance with standards like PCI DSS or CIS.

**Severity Levels:**

* **Critical**: Requires immediate action (e.g., unauthorized access detected).
* **High**: Significant risk but not actively exploited (e.g., overly permissive IAM roles).
* **Medium**: Misconfigurations that may expose resources in the future.
* **Low**: Issues with minimal risk (e.g., redundant settings).

**Example Findings:**

* **Crypto-mining Threat**: Identifies VMs running crypto-mining software.
* **Public Storage Bucket**: Alerts you when sensitive data is publicly exposed.

### **Sources and Detectors**

SCC aggregates findings from multiple **sources** using both **Google-native detectors** and **custom detectors**.

**Google-Provided Sources and Detectors:**

* **Cloud Asset Inventory**: Tracks assets and configuration changes.
* **Security Health Analytics**: Monitors for compliance and security risks.
* **Event Threat Detection**: Identifies suspicious activities, like compromised accounts or malware.
* **Container Threat Detection**: Monitors Kubernetes workloads for signs of attacks or misconfigurations.

**Custom Detectors:**

In SCC Premium, organizations can build **custom detectors** to track specific security events. These can be tailored to business-specific risks, such as unauthorized modifications to sensitive workloads.

**Example Use Case of Custom Detectors:**

* **Scenario:** Detect unauthorized changes to firewall rules.
* **Solution:** Create a custom detector to alert when a firewall rule opens a port to the internet.

### **SCC Insights and Dashboard Views**

SCC offers **dashboards** to visualize findings, asset trends, and compliance status across multiple projects. You can customize these views for specific audiences, such as:

* **Security Teams**: View real-time threat detection and incident status.
* **Compliance Teams**: Track benchmark violations and prepare audit reports.
* **Executives**: Get a high-level overview of the organization’s security posture.

**Key Metrics to Monitor:**

* **Top Findings by Severity**: Prioritize the most critical issues.
* **Assets at Risk**: Identify the number of vulnerable assets across projects.
* **Compliance Trends**: Track the organization's progress toward meeting benchmarks.

### **Real-World Application: Monitoring with SCC**

**Scenario:**\
A security team wants to monitor **cloud storage buckets** for unauthorized public access and automate a response if any new public buckets are created.

**Solution:**

1. **Use Asset Inventory** to track storage buckets and identify which ones are public.
2. **Create a Custom Detector** to trigger an alert whenever a bucket becomes publicly accessible.
3. **Automate Response:** Use a **Cloud Function** to remove public access automatically when the alert triggers.

### **Hands-On Lab: Exploring SCC Components**

In this hands-on lab, participants will:

1. **Enable SCC for their GCP project.**
2. **Explore Asset Inventory**: Search for specific resources (e.g., VMs or storage buckets) in their project.
3. **Investigate Findings**: Filter findings by severity and type to identify potential risks.
4. **Create a Custom Detector**: Build a detector to monitor firewall changes.
5. **Visualize Data**: Use the SCC dashboard to create a view that shows compliance status across all assets.
