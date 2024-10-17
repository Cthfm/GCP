# Log Forwarding (Sinks)

High-Level Overview:

* **Routing**: Determines where logs are sent. They can go to different destinations, such as **Cloud Logging buckets**, **BigQuery**, or **Pub/Sub**. Routing allows you to manage and control the flow of logs.
* **Storage**: Logs are stored for later use, often in **buckets**. You can create different buckets for organizing or archiving logs.

#### Key Components:

1. **Log Router**:
   * Every log entry passes through the **Log Router**. This component ensures that logs are properly filtered and routed to their destination.
   * It uses **sinks** to determine where logs go. A **sink** can route logs to **Cloud Storage**, **BigQuery**, **Pub/Sub**, or even another project.
   * The Log Router can temporarily store logs to avoid data loss in case of outages. However, this is not a permanent storage solution, and logs are discarded if they can't be routed properly.
2. **Sinks**:
   * Sinks control where logs go and what gets routed. You can create sinks to:
     * Send logs to **storage buckets** for long-term retention.
     * Send logs to **BigQuery** for analysis.
     * Route logs to **Pub/Sub** for third-party integrations.
   * **Predefined sinks**: Every Google Cloud project has predefined sinks like `_Default` and `_Required`, which automatically route logs to certain locations.
   * **Custom sinks**: You can create your own sinks to decide where specific logs should go. You can set **inclusion filters** to choose which logs to include and **exclusion filters** to remove logs that don’t need to be routed.
3. **Destinations**:
   * **Cloud Logging bucket**: Stores logs for analysis.
   * **BigQuery dataset**: Stores logs for big data analysis and queries.
   * **Cloud Storage**: Stores logs in JSON format, which can be used for long-term archiving.
   * **Pub/Sub**: Used to stream logs to other systems or external platforms.

#### Example:

Imagine you want to analyze login attempts in your Google Cloud environment. You could set up a **sink** with a filter to route only **authentication logs** to **BigQuery**. This allows you to focus on those logs and perform detailed queries.

#### Sinks, Filters, and Exclusion:

* **Inclusion Filters**: Specify the types of logs to include. For example, you could filter to only route logs related to **Compute Engine** instances.
* **Exclusion Filters**: Remove logs from the flow. For example, if you're not interested in specific debug logs, you can exclude them.

#### Log Storage Options:

* **Buckets**: Logs are stored in **log buckets** inside Cloud Logging. There are predefined buckets (`_Required` for critical logs, `_Default` for general logs).
  * You can also create **user-defined buckets** for specific purposes like long-term retention or organization of logs.
  * Logs stored in these buckets can be viewed, queried, and managed in **Cloud Logging**.

#### Use Cases:

* **Compliance**: Store logs that need to be retained for regulatory purposes.
* **Analysis**: Route logs to BigQuery to perform queries and find trends.
* **Third-party systems**: Send logs to Pub/Sub, where they can be consumed by external systems like SIEM tools.
