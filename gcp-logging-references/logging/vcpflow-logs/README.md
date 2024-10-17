# VCPFlow Logs

## Overview

VPC Flow Logs provide detailed information about network traffic within your Google Cloud environment. These logs help with **network monitoring**, **security analysis**, **forensics**, and **cost optimization** by recording data on packets sent to and from your VM instances, including Google Kubernetes Engine (GKE) nodes, Cloud VPN tunnels, and VLAN attachments.

### Key Points About VPC Flow Logs

1. **What Do VPC Flow Logs Record?** VPC Flow Logs capture a sample of network traffic (packets) moving through VM instances and network interfaces. These logs focus on **5-tuple IP connections** (source and destination IPs, source and destination ports, and the protocol). They record both inbound and outbound flows.
2. **Use Cases**:
   * **Network Monitoring**: Helps you keep track of network activity and performance, allowing you to:
     * Monitor traffic between regions, zones, and external networks.
     * Diagnose and troubleshoot issues.
   * **Cost Optimization**: By understanding your network usage, you can optimize traffic to reduce expenses. For example, analyzing high-traffic areas and determining if they can be optimized.
   * **Network Forensics**: In case of a security incident, you can trace which IPs communicated, what traffic was exchanged, and when, to help in investigations.
3. **How VPC Flow Logs Work**:
   * **Aggregation**: Flow logs are aggregated into entries based on time intervals. This reduces the size of log data while still providing useful insights.
   * **Sampling**: VPC Flow Logs only capture a **sample** of network traffic, not every single packet. This sampling is dynamic and depends on the load of the VM’s physical host at the time.
     * **Primary Sampling**: Automatically happens when packets leave/enter a VM. You can’t control this, and it’s based on the volume of traffic.
     * **Secondary Sampling**: You can configure this for more granular control, adjusting how much of the initially captured traffic is kept. For example, setting a 100% rate captures all logs from the primary sampling, while lower rates capture less.
4. **Interaction with Firewall Rules**:
   * **Egress** (outgoing) traffic: Flow logs are captured before egress firewall rules are applied. So, even if a firewall rule blocks the traffic, it may still be logged.
   * **Ingress** (incoming) traffic: Flow logs are captured after ingress firewall rules are applied. If an ingress firewall rule blocks traffic, it won’t appear in the logs.
5. **Flow Log Entry Components**: A flow log entry contains the following key information:
   * **Source and destination IPs**
   * **Source and destination ports**
   * **Protocol used** (TCP, UDP, ICMP, etc.)
   * **Metadata**: This can include additional information, such as the geographic location of the IPs or the names of VMs and regions involved in the traffic.
6. **Filtering and Customization**:
   * **Filters**: You can filter flow logs by specific criteria (e.g., by VM, subnet, or metadata). This allows you to focus on relevant data and avoid unnecessary logs.
   * **Metadata Annotations**: You can choose to include or exclude metadata, such as region or VM names, to save storage space.
7. **Storage and Retention**: By default, VPC Flow Logs are stored in **Cloud Logging** for 30 days. You can customize retention periods or export logs to **BigQuery**, **Cloud Storage**, or other supported destinations for long-term storage or advanced analysis.

### How VPC Flow Logs Help:

* **Visibility**: Gain visibility into how your network is being used, from traffic patterns between regions to top users of your network resources.
* **Troubleshooting**: When something goes wrong, such as network slowdowns or security incidents, flow logs allow you to pinpoint what traffic occurred and what might have caused the issue.
* **Security Analysis**: You can identify potential threats by analyzing traffic flows, determining which IPs communicated and whether any unusual traffic patterns exist.
* **Capacity Planning**: VPC Flow Logs help track traffic growth, allowing you to forecast future network needs and optimize resources accordingly.

### Examples of VPC Flow Log Use:

* **Detecting Anomalous Traffic**: If unusual traffic patterns or unexpected communications occur between VMs, you can use VPC Flow Logs to identify the source and investigate further.
* **Analyzing Traffic to Specific Regions**: For compliance or cost-saving purposes, you may need to monitor traffic flows to specific countries or regions.
* **Monitoring Internal Network Flows in GKE**: You can enable **Intranode visibility** in GKE clusters to monitor traffic between Pods on the same node.
