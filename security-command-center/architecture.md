# Architecture

### **SCC Architecture Overview**

SCC integrates with multiple Google Cloud services to provide continuous asset visibility, security monitoring, and threat detection. Understanding the architecture will help you align SCC with your organization’s security needs.

**Core Architectural Components:**

1. **SCC Organization-Level Integration**
   * SCC is typically enabled at the **organization level** to monitor all projects under a single organization.
   * Findings and assets are aggregated across projects, providing a unified view.
2. **Sources and Collectors**
   * SCC ingests data from **native Google sources** (e.g., Event Threat Detection, Security Health Analytics).
   * It also allows you to integrate **custom sources** and **third-party security solutions** (via Chronicle, Splunk).
3. **IAM-Based Access Control**
   * SCC relies heavily on **IAM permissions** to control access to resources and findings.
   * SCC data can be exported to **BigQuery or Pub/Sub** for advanced analytics and alerts.

### **Enabling SCC for Your Organization or Project**

To use SCC, it must first be enabled at either the **organization** or **project level**. The organization-level setup is recommended for enterprises to maintain centralized control.

**Steps to Enable SCC:**

1. **Navigate to Security Command Center**
   * In the Google Cloud Console, search for **"Security Command Center"** and open the dashboard.
2. **Select the Scope of SCC**
   * Choose between **organization-wide** or **project-level** SCC.
   * If using multiple projects, select **organization-level integration** for broader visibility.
3. **Enable SCC API**
   * Go to **APIs & Services** in the Console and enable the **Security Command Center API**.
4. **Enable Native Security Sources**
   * Go to SCC > **Settings** > **Security Sources** and enable:
     * **Event Threat Detection**
     * **Security Health Analytics**
     * **Container Threat Detection** (if using GKE)

### **IAM Roles and Permissions for SCC**

IAM roles are critical for managing access to SCC findings and configurations. Implementing **least privilege access** is essential for securing SCC.

**Predefined Roles for SCC:**

* **Security Center Admin (`roles/securitycenter.admin`)**
  * Full access to configure SCC settings and manage findings.
* **Security Center Findings Viewer (`roles/securitycenter.findingsViewer`)**
  * Read-only access to view SCC findings.
* **Security Center Analyst (`roles/securitycenter.analyst`)**
  * View findings and export data to BigQuery or Pub/Sub.
* **Security Center Asset Viewer (`roles/securitycenter.assetsViewer`)**
  * View asset inventories without access to findings.

**Assigning Roles:**

1. Go to **IAM & Admin** in the Console.
2. Select the appropriate **user or service account**.
3. Assign **one or more SCC-specific roles** based on the user's responsibilities.

### **SCC Service Accounts**

**Service accounts** are used by SCC to interact with other Google services like BigQuery, Cloud Functions, and Pub/Sub. Properly configuring these accounts ensures smooth operation and data flow.

**Steps to Create a Service Account:**

1. **Navigate to IAM & Admin** > **Service Accounts**.
2. **Create a new service account** for SCC.
3. **Assign the required roles** (e.g., `securitycenter.viewer`, `pubsub.publisher`).
4. **Generate and download the key** if needed for external integrations.

**Best Practices for Service Accounts:**

* Use **one service account per security function** (e.g., one for Pub/Sub alerts, one for BigQuery exports).
* Regularly **rotate service account keys** to minimize the impact of key exposure.
* Follow the **principle of least privilege** by assigning only the roles necessary for each service account.

### **Setting Up SCC in a Multi-Project Environment**

Organizations often have multiple projects under one GCP organization. SCC can be configured to **monitor all projects** and provide a consolidated security view.

**Steps to Enable SCC for Multi-Project Environments:**

1. **Enable SCC at the Organization Level.**\
   This ensures all projects under the organization are covered.
2. **Configure Resource Visibility.**\
   In SCC > **Settings**, define which projects are visible to SCC. Use **inclusion and exclusion filters** if needed.
3. **Set Organization Policies.**
   * Use **Organization Policy Service** to enforce security baselines across projects.
   * Example: Block public IPs on all VMs.
4. **Assign IAM Roles for Each Project.**\
   Ensure project owners or administrators have the appropriate **SCC roles** (e.g., `securitycenter.assetsViewer`) for visibility.

### **Integrating SCC with Other GCP Services**

SCC integrates with **BigQuery, Pub/Sub, and Cloud Logging** for advanced analytics and automation.

**BigQuery Integration:**

Export SCC findings to **BigQuery** for **custom queries** and **dashboards**.

* Use SQL queries to analyze trends in findings.
* Example Query: Track changes in IAM roles across projects.

**Pub/Sub Integration:**

Configure SCC to send **real-time alerts** via **Pub/Sub**.

* Use **alert triggers** to initiate automated workflows (e.g., Cloud Functions to quarantine VMs).

**Cloud Logging Integration:**

Export SCC logs to **Cloud Logging** for **long-term storage** and **auditing**.

* Set up **custom dashboards** in Cloud Monitoring for leadership reports.

### **Best Practices for SCC Deployment**

* **Use Organization-Level SCC Setup**: Centralize management and visibility.
* **Leverage IAM Permissions Wisely**: Use **predefined roles** and follow **least privilege principles**.
* **Automate Workflows with Pub/Sub and Cloud Functions**: Ensure quick responses to incidents.
* **Monitor SCC Logs Regularly**: Export logs to Cloud Logging and **set up alerts** for critical events.
* **Review and Update SCC Configurations Periodically**: Security requirements evolve, so make it a habit to **audit settings** every quarter.
