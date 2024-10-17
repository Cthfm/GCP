# GCP NGFW

## Overview

Firewall Rules Logging is a feature in Google Cloud that allows you to audit, verify, and analyze the behavior of your firewall rules. By enabling logging for specific firewall rules, you can gather detailed insights into the traffic that is either allowed or denied by those rules. This logging capability helps you understand how your firewall rules are functioning, whether they are effectively blocking or allowing traffic as intended, and how many connections are being affected by each rule.

### Key Benefits of Firewall Rules Logging

1. **Audit and Verification**: You can confirm that your firewall rules are working as expected by reviewing the logs. For example, if you have a rule that is meant to block certain traffic, you can check the logs to ensure that the rule is indeed denying those connections.
2. **Traffic Analysis**: Firewall logs provide detailed information about the traffic hitting your firewall rules. This includes data on both allowed and denied connections, helping you assess the impact of your rules on the overall network traffic.
3. **Troubleshooting**: If network issues arise, Firewall Rules Logging can help you identify whether a specific firewall rule is causing problems. Logs can show whether traffic is being blocked or allowed and help you trace the source of any connectivity issues.
4. **Visibility**: Firewall logs provide visibility into the traffic flowing through your VPC network, including traffic involving Compute Engine VM instances, Google Kubernetes Engine (GKE) clusters, and App Engine flexible environment instances.

### How Firewall Rules Logging Works

Firewall Rules Logging must be enabled individually for each firewall rule where you want to capture traffic logs. You can enable logging for any type of rule, whether it’s an **allow** or **deny** rule, and for both **ingress** (incoming) and **egress** (outgoing) traffic.

When logging is enabled for a rule, Google Cloud generates **connection records** each time the rule processes traffic. These logs capture essential details, such as:

* **Source and destination IP addresses**
* **Source and destination ports**
* **Protocol used (e.g., TCP, UDP)**
* **Timestamp (date and time)**
* **Firewall rule reference** (the rule applied to the traffic)
* **Disposition**: Whether the traffic was allowed or denied

Logs can be viewed in **Cloud Logging**, and you can export them to other destinations supported by Cloud Logging, such as Cloud Storage, BigQuery, or Pub/Sub for long-term analysis, storage, or integration with third-party tools.

### Key Specifications of Firewall Rules Logging

1. **VPC Network Only**: Firewall Rules Logging is only supported for firewall rules in VPC networks. It is not available for legacy networks.
2. **TCP and UDP Logging**: Logging is limited to traffic using TCP and UDP protocols. Other protocols, like ICMP, cannot be logged through Firewall Rules Logging. If you need to log traffic for other protocols, you can use **Packet Mirroring**.
3. **Implied Rules Excluded**: Firewall Rules Logging cannot be enabled for the default **implied deny ingress** or **implied allow egress** firewall rules that Google Cloud automatically creates.
4. **Perspective**: Logs are created from the perspective of the VM instance. This means that log entries are only generated if the rule is applied to traffic coming **to** or **from** the VM and only when logging is enabled for the rule.
5. **Logging Limits**: The number of connections that can be logged in a given time period depends on the **machine type** of the VM. If too much traffic is processed, logging may not capture every connection, but Google Cloud attempts to log connections on a best-effort basis.

### Example Use Cases and Log Details

Let’s explore several practical examples of how Firewall Rules Logging works:

**Egress Deny Example**

In this scenario, we have two VM instances, **VM1** in the **us-west1** region and **VM2** in the **us-east1** region. The firewall rules are as follows:

* **Rule A**: Egress deny rule, which blocks any traffic from **VM1** to **VM2** on **TCP port 80**.
* **Rule B**: Ingress allow rule, which allows traffic from **VM1** to **VM2** on **TCP port 80**.

Both rules have logging enabled.

* If **VM1** tries to connect to **VM2** on TCP port 80, **Rule A** applies and denies the traffic. A log entry is created showing that traffic from VM1 was **denied**.
* Since **Rule A** blocked the traffic, **Rule B** (ingress allow) is never triggered, so no log is generated for VM2.

The log entry would include fields like:

* **Source IP**: 10.10.0.99 (VM1)
* **Destination IP**: 10.20.0.99 (VM2)
* **Port**: 80
* **Protocol**: TCP (protocol number 6)
* **Disposition**: DENIED
* **Rule Reference**: `network:example-net/firewall:rule-a`
* **Direction**: Egress (outbound from VM1)

**Egress Allow and Ingress Allow Example**

In this scenario, both egress and ingress traffic between **VM1** and **VM2** on TCP port 80 is allowed by the firewall rules:

* **Rule A**: Egress allow rule for traffic from **VM1** to **VM2** on TCP port 80.
* **Rule B**: Ingress allow rule for traffic from **VM1** to **VM2** on TCP port 80.

Both rules have logging enabled.

* When **VM1** initiates a connection to **VM2**, a log entry is generated by **Rule A** showing that traffic from VM1 was **allowed**.
* A second log entry is generated by **Rule B** showing that traffic to **VM2** was **allowed**.

Each log entry would include fields similar to the previous example, but the **disposition** would be **ALLOWED** instead of **DENIED**.

**Ingress from the Internet Example**

In this scenario, traffic from an external IP address (e.g., a public IP on the internet) is attempting to reach a VM inside the VPC:

* **Rule C**: Ingress allow rule for TCP port 80 from any source IP address (0.0.0.0/0).
* **Rule D**: Egress deny rule for all protocols and destinations.

If a system with IP address **203.0.113.114** tries to connect to **VM1** on TCP port 80:

* **Rule C** applies and allows the traffic. A log entry is created showing that traffic from the external IP was allowed.
* Even though **Rule D** is an egress deny rule, it does not block **replies** to allowed traffic, because firewall rules are stateful (i.e., once an incoming connection is allowed, responses are also allowed). Therefore, no log entry is generated for Rule D.

#### Log Entry Structure

Each firewall log entry contains the following key fields:

* **Connection**: The 5-tuple that defines the flow (source IP, source port, destination IP, destination port, and protocol).
* **Disposition**: Whether the connection was **ALLOWED** or **DENIED**.
* **Rule Reference**: A reference to the firewall rule that applied to the connection (e.g., `network:{network_name}/firewall:{firewall_name}`).
* **Instance Details**: Information about the VM instance, such as project ID, instance name, region, and zone.
* **VPC Details**: Information about the VPC network, including VPC name and subnetwork details.
* **Remote Instance and VPC Details**: If the remote endpoint is a VM, details about that VM and its network.

## Firewall Rules Permissions

The following is a link to permissions need to modify firewall rules

{% embed url="https://cloud.google.com/firewall/docs/using-firewall-rules-logging" %}
