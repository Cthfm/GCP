# Google Cloud Logging Overview

**Google Cloud Logging** is a managed service that enables you to store, search, monitor, and analyze logs from your Google Cloud resources and applications. For a threat hunter, these logs are the foundation of identifying malicious behavior and unusual patterns in the environment.

**Key Log Types**&#x20;

1. **Audit Logs**
   * **Admin Activity Logs**: Capture operations that modify the configuration of GCP services. This includes creating, updating, or deleting resources, as well as IAM changes like role assignments.
   * **Data Access Logs**: Record actions taken on resources (e.g., reading or writing data in Cloud Storage). These are not enabled by default for all services but should be enabled for sensitive resources.
   * **System Event Logs**: Capture system-driven events that impact resources (e.g., auto-scaling events).
   * **Policy Denied Logs**: Capture actions that are denied due to security policies, which could indicate unauthorized attempts to access resources.
2. **VPC Flow Logs**
   * These logs capture metadata about the IP traffic to and from your Google Cloud Virtual Private Cloud (VPC) networks. VPC Flow Logs are critical for detecting potential lateral movement, data exfiltration, or unusual network traffic.
3. **Access Transparency Logs**
   * These logs provide visibility into when Google employees access your data for support or maintenance purposes. Although rare, monitoring these logs ensures accountability and transparency.
4. **Firewall Rules Logging**
   * Logs firewall rule evaluations and whether the traffic is allowed or blocked. Monitoring this helps to identify unauthorized access attempts or improperly configured firewall rules.
