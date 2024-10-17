# VPC Flow Log Records

### Overview

VPC Flow Logs are an invaluable tool within Google Cloud that provide detailed insights into the traffic flowing through your Virtual Private Cloud (VPC) network. By recording information about the network packets sent and received by your virtual machine (VM) instances, Google Kubernetes Engine (GKE) nodes, VLAN attachments, and Cloud VPN tunnels, VPC Flow Logs offer crucial data for network monitoring, security analysis, troubleshooting, and cost optimization. Understanding the structure and customizable nature of these logs allows you to tailor the insights you gather, while optimizing for both performance and storage efficiency.

### Detailed Overview of VPC Flow Logs Record Format

Each VPC Flow Log entry is composed of **base fields** and optional **metadata fields**, providing two layers of data:

1. **Base Fields**: These are essential, always-present fields that capture core information directly from packet headers. Base fields cannot be omitted as they provide the foundational data required to describe network traffic.
2. **Metadata Fields**: These are optional, offering additional context about the network flow. Metadata fields can be included or excluded depending on your requirements for analysis and storage efficiency. By managing metadata fields effectively, you can balance the depth of information with cost considerations.

### **Base Fields (Essential Network Information)**

The base fields provide core data that describes each network flow, enabling you to understand the who, what, where, and when of network traffic within your VPC. Let’s break down each of the base fields:

*   **Connection (IpConnection)**: This is a **5-tuple** that identifies the network flow. It includes the source and destination IP addresses, source and destination ports, and the protocol used (e.g., TCP or UDP). The connection field is crucial because it describes the core elements of the traffic flow between two endpoints. The details of the connection field are:

    * **Source IP**: The IP address from which the traffic originated.
    * **Destination IP**: The IP address to which the traffic is being sent.
    * **Source Port**: The port number on the source device used for communication.
    * **Destination Port**: The port number on the destination device receiving the traffic.
    * **Protocol**: The protocol used for the communication, identified by its Internet Assigned Numbers Authority (IANA) protocol number (e.g., TCP is 6, UDP is 17).

    This data is critical for identifying and tracking specific traffic patterns, determining the source and destination of traffic, and evaluating which protocols are being used.
* **Reporter**: This field indicates which entity is reporting the traffic. It can be either the source (`SRC`), the destination (`DEST`), or a gateway (`SRC_GATEWAY` or `DEST_GATEWAY`). The reporter field helps you identify the point in the network where the traffic was captured, which is useful for analyzing traffic as it crosses various boundaries in your network. For example, if traffic is flowing through a VPN tunnel or an interconnect attachment, the gateway will report the traffic.
* **RTT (Round Trip Time)**: This field is available only for **TCP** traffic and measures the time taken for a packet to travel from the source to the destination and back. It is calculated based on the time elapsed between sending a sequence number (`SEQ`) and receiving its acknowledgment (`ACK`). The RTT includes both network latency and any processing delays introduced by the application. This field is crucial for monitoring network performance and diagnosing latency issues. For example, unusually high RTT values could indicate network congestion, inefficient routing, or overloaded servers.
* **Bytes Sent**: This field records the total number of bytes transmitted from the source to the destination. It provides a quantitative measure of data transfer in each network flow. This field is helpful for tracking bandwidth usage and identifying high-traffic flows that may require optimization or further scrutiny, such as identifying the “top talkers” in your network who are consuming the most bandwidth.
* **Packets Sent**: This field tracks the number of packets transmitted from the source to the destination. Understanding the number of packets sent, in conjunction with the bytes sent, can help you gain insight into how efficiently data is being transferred. For example, a high number of small packets could indicate inefficient communication between applications, while a smaller number of large packets might suggest more optimal data transfers.
* **Start Time and End Time**: These fields capture the timestamps (in RFC 3339 format) of when the first and last packets were observed during the log aggregation interval. These timestamps help determine the duration of the flow and can be useful for correlating with other events, such as identifying when a network anomaly or security incident started and ended. This is critical for forensic investigations where timing is crucial.

### **Metadata Fields (Optional Enrichments)**

Metadata fields provide **contextual information** that can enrich your understanding of network flows. These fields, unlike the base fields, are approximations and may sometimes be incomplete or inaccurate, but they offer additional insights that can be valuable for more detailed analysis. The metadata fields can be customized, meaning you can choose to enable or disable specific metadata to reduce storage costs or focus on the data most relevant to your needs.

Here are some key metadata fields you can include:

*   **Source and Destination Instance Details**:

    * **Project ID**: The ID of the Google Cloud project where the VM instance resides.
    * **VM Name**: The name of the VM instance involved in the flow.
    * **Zone and Region**: The geographic zone or region where the VM is located.

    This data is helpful for identifying which cloud resources are generating or receiving traffic, linking specific network flows to particular VMs, and understanding how different regions or zones are being utilized.
*   **Source and Destination GKE Details**: If the traffic involves Google Kubernetes Engine (GKE), this metadata includes details about the GKE environment, such as:

    * **Cluster Name**: The name of the GKE cluster.
    * **Pod Name**: The name of the Kubernetes Pod involved in the traffic.
    * **Service Details**: Information about Kubernetes services, such as the name and namespace of the service. If there are multiple services involved, the record may indicate this with a `MANY_SERVICES` marker.

    These details are useful for troubleshooting issues within Kubernetes clusters and understanding how microservices and containers interact within the network.
*   **Source and Destination VPC Details**:

    * **VPC Name**: The name of the Virtual Private Cloud (VPC) network.
    * **Subnetwork Name and Region**: The subnet and region within the VPC where the traffic originated or was destined.

    These fields help map traffic across different VPC networks, which can be important for understanding the layout of your cloud infrastructure and for ensuring that your VPC segmentation and security policies are being followed.
*   **Geographic Details**: For traffic involving public IP addresses (i.e., traffic that enters or leaves the VPC), geographic metadata provides information such as:

    * **Country**: The country of the external IP (based on [ISO 3166-1 Alpha-3](https://en.wikipedia.org/wiki/ISO\_3166-1\_alpha-3) codes).
    * **City**: The city of the external IP.
    * **Region**: The region (within a country) of the external IP.

    This metadata is useful for tracking the geographic origin of traffic, which can be essential for compliance, geofencing, or detecting unusual traffic patterns (such as traffic from unexpected or high-risk regions).
*   **Gateway Details**: If traffic passes through a gateway, such as a VPN tunnel or an interconnect attachment, this field captures details about the gateway, such as:

    * **Gateway Name**
    * **Gateway Type** (e.g., VLAN attachment, VPN tunnel)

    This field is particularly useful for tracking traffic between on-premises environments and your Google Cloud resources, providing insight into how your hybrid or multi-cloud network is functioning.
* **Load Balancing Details**: If the traffic passes through a load balancer, this field captures information about the type of load balancer (e.g., Application Load Balancer, Network Load Balancer) and whether the flow originated from the client or backend side of the load balancer. It also includes details such as the forwarding rule name and URL map (for HTTP/HTTPS traffic). This information is valuable for diagnosing load balancer issues and optimizing traffic distribution.

### **Customizing Log Records with Filters**

VPC Flow Logs allows you to create **filters** to control which logs are generated. This is useful for reducing storage costs, focusing on specific types of traffic, and ensuring that you’re capturing only the most relevant data for analysis. Filters are created using a custom expression language called **CEL** (Common Expression Language), which allows you to define complex logical conditions.

For example:

*   **Limit Logs to a Specific VM**:

    ```bash
    gcloud compute networks subnets update my-subnet \
    --logging-filter-expr="(src_instance.vm_name == 'my-vm' && reporter=='SRC') || (dest_instance.vm_name == 'my-vm' && reporter=='DEST')"
    ```

    This filter ensures that only traffic from or to a specific VM (`my-vm`) is logged, helping you focus on traffic from critical resources.
*   **Limit Logs to a Specific IP Range**:

    ```bash
    gcloud compute networks subnets update my-subnet \
    --logging-filter-expr="inIpRange(connection.src_ip, '10.0.0.0/8')"
    ```

    This filter logs traffic originating from IP addresses in the `10.0.0.0/8` subnet, useful for monitoring internal traffic within your VPC.
*   **Capture Only External Traffic**:

    ```bash
    gcloud compute networks subnets update my-subnet \
    --logging-filter-expr '!(has(src_vpc.vpc_name) && has(dest_vpc.vpc_name))'
    ```

    This filter captures only traffic that crosses the VPC boundary, filtering out internal VPC traffic. This can be useful for monitoring traffic that interacts with external networks, such as internet traffic or traffic to and from on-premises environments.

By using these filters, you can ensure that your VPC Flow Logs are aligned with your specific monitoring, security, and performance goals, while also keeping storage and logging costs under control.

{% embed url="https://cloud.google.com/vpc/docs/about-flow-logs-records" %}
