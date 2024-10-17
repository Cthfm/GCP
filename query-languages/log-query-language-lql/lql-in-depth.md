# LQL In Depth

The Logging Query Language (LQL) in Google Cloud is a specialized syntax designed to query and filter log entries within Cloud Logging. Its purpose is to help you find specific logs, create custom filters, or build log-based metrics and sinks (data pipelines) by leveraging the extensive logging capabilities provided by Google Cloud. Here’s a detailed explanation to help clarify key concepts and functionalities.

#### **Where You Use LQL**

You use the Logging Query Language (LQL) in several contexts:

* **Logs Explorer**: A GUI tool within the Google Cloud Console that allows you to interactively query logs.
* **Logging API**: You can programmatically query logs via RESTful API.
* **gcloud CLI**: The command-line interface allows you to execute logging queries via terminal commands.

#### **Basic Structure**

At its core, a query is a Boolean expression—meaning it evaluates to either true or false. The query acts as a filter that narrows down which logs you retrieve from a larger set. You can build queries by specifying conditions that logs must meet, often comparing values in the log to other values.

For instance, let’s start with the simplest form:

```plaintext
resource.type = "gce_instance"
```

This query looks for log entries related to **Google Compute Engine** instances (gce\_instance). Here, `resource.type` is a field, `=` is a comparison operator, and `"gce_instance"` is a value.

#### **Logical Operators**

To build more complex queries, you can use **logical operators** like `AND`, `OR`, and `NOT` to combine or negate conditions:

* **AND** means both conditions must be true.
* **OR** means at least one condition must be true.
* **NOT** excludes entries that meet the specified condition.

Example:

```plaintext
resource.type = "gce_instance" AND severity >= "ERROR"
```

This query will return log entries where:

* The log's resource type is Google Compute Engine, **and**
* The log’s severity (level of importance) is **at least** “ERROR” (which includes errors, critical issues, and alerts).

#### **Implicit AND in Logs Explorer**

In the **Logs Explorer**, each new line you add is automatically combined with an `AND` operator. For example:

```plaintext
resource.type = "gce_instance"
severity = "ERROR"
```

The query is implicitly converted to:

```plaintext
resource.type = "gce_instance" AND severity = "ERROR"
```

If you move this query to another context (e.g., logging API), you must explicitly add the `AND`.

#### **Combining AND and OR**

If you want to mix `AND` and `OR` operators, you must **use parentheses** to clarify how the expressions should be evaluated. Consider this example:

```plaintext
resource.type = "gce_instance" AND (severity = "ERROR" OR textPayload : "error")
```

This query returns logs related to Compute Engine where either:

* The severity level is ERROR, or
* The log contains the word "error" in its `textPayload` field.

The parentheses ensure that the OR condition is grouped together and properly evaluated.

#### **Common Comparisons**

In addition to `AND` and `OR`, you can use a range of comparison operators:

* `=`: Equals
* `!=`: Not equal
* `:`: Substring match (e.g., `"field contains string"`)
* `>`, `<`, `>=`, `<=`: Greater than, less than, and so on (for numeric values)
* `=~`: Regular expression match (for complex pattern searching)

#### **Sample Queries**

Let’s look at more practical examples to showcase how the query language can be applied.

**Query 1: Finding Logs with Specific Error Severity**

```plaintext
resource.type = "gce_instance" AND severity >= "ERROR"
```

* This query will return all logs from Compute Engine VMs that have a severity of **at least ERROR**.

**Query 2: Excluding Specific Words**

```plaintext
resource.type = "gce_instance" AND severity >= "ERROR" AND NOT textPayload : "robot"
```

* This query will return logs from Compute Engine with severity ≥ ERROR but will **exclude** any log entries that contain the word “robot” in the `textPayload` field.

**Query 3: Using Regular Expressions**

```plaintext
jsonPayload.message =~ "error.*timeout"
```

* This uses a regular expression (`=~`) to search for log entries where the `jsonPayload.message` field matches the pattern “error” followed by any characters and then the word “timeout.”

#### **Field Path Identifiers**

A field path identifier is how you reference fields within the log entry. The syntax follows a dot notation, where you navigate through nested fields, like:

* `resource.type`
* `jsonPayload.error`
* `httpRequest.latency`

In cases where field names contain special characters (e.g., forward slashes `/`), you need to enclose those parts in double quotes, like so:

```plaintext
labels."compute.googleapis.com/resource_id"
```

#### **Handling Missing Fields**

If you query a field that doesn’t exist in a log entry, it’s either considered **missing** or **undefined**:

* Missing fields silently fail and return no results.
* Undefined fields generate an error.

You can test for the existence of a field using `:*`. For example:

```plaintext
operation.id : *
```

This checks if the field `operation.id` is present in the log entry.

#### **Optimizing Queries**

Google Cloud Logging supports indexed fields for faster queries. Indexed fields include common ones like `resource.type`, `logName`, `severity`, and `timestamp`. By specifying indexed fields in your queries, you can reduce query times. For example:

```plaintext
resource.type = "gce_instance" AND logName = "projects/my-project-id/logs/cloudaudit.googleapis.com%2Factivity"
```

In this query, both `resource.type` and `logName` are indexed, which means the system can quickly find the relevant logs.

#### **Time-Based Queries**

You can also filter logs by time ranges. The timestamps must be in ISO 8601 or RFC 3339 format. Example:

```plaintext
timestamp >= "2024-10-01T00:00:00Z" AND timestamp <= "2024-10-01T12:00:00Z"
```

This query returns logs between 12:00 AM and 12:00 PM UTC on October 1, 2024.

#### **Regular Expressions for Advanced Search**

Regular expressions (`=~`, `!~`) allow you to search for patterns within string fields. Regular expressions are case-sensitive, and they don’t normalize strings (e.g., "kubernetes" is not the same as "KUBERNETES"). Examples of regex searches include:

```plaintext
labels.pod_name =~ "(?i)foo"  # Case-insensitive match for "foo"
logName =~ "^projects/my-project-id/logs/somelog"  # Starts with pattern
```

#### **Efficient Querying Tips**

* **Use indexed fields**: Focus queries on indexed fields like `resource.type`, `logName`, and `severity`.
* **Limit the time range**: Narrow down queries to specific timestamps to reduce the number of logs returned.
* **Minimize global or substring searches**: Avoid queries that search across all fields if you can be more specific.

#### **Built-In Functions**

LQL includes a few built-in functions to make querying easier, like:

* **`log_id([LOG_ID])`**: Matches log entries by log ID.
* **`sample([FIELD], [FRACTION])`**: Selects a fraction of the total log entries for sampling.
* **`ip_in_net([FIELD], [SUBNET])`**: Tests whether an IP address falls within a specified subnet.

#### **Example Using Sample Function**

```plaintext
logName = "projects/my-project/logs/syslog" AND sample(insertId, 0.25)
```

This query samples 25% of the log entries from the syslog logs.
