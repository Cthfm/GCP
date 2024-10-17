# Split Logs

## Overview

When Cloud Logging splits an oversized audit log entry, it divides the data into several smaller log entries, each containing parts of the original entry. This happens when the size of a single log entry exceeds Cloud Logging’s limits.

### Key Points to Understand About Split Audit Log Entries

1. **Split Fields**: When a log is split, a `split` field is added. This field includes:
   * **split.uid**: A unique identifier that connects all log entries split from the same original entry.
   * **split.index**: The position of this log entry in the series of split entries (starting from 0).
   * **split.totalSplits**: The total number of split log entries created from the original entry.
2. **Fields in Split Entries**:
   * Most fields, **except the `protoPayload` field**, are duplicated in each split log entry.
   * The **`protoPayload` field** contains key information about the request, response, metadata, and more. Only some of its subfields (like metadata, request, and response) are split across multiple entries if they are too large.
   * For example, if the `request` field contains a long string, it may be divided into pieces across multiple split log entries.
3. **Splitting of Repeated Fields**:
   * If a field contains repeated values, the values may be broken up and distributed across split entries.
   * Example: A list of values like `["foo", "bar", "baz"]` could be split into multiple parts across entries, such as `["foo", "ba"]` in one entry and `["", "r", "baz"]` in another.

### Reassembling Split Log Entries

To reassemble the original log from its split pieces, follow these steps:

1. **Sort by `split.index`**: Start by sorting the split log entries based on the `split.index` field, from 0 upwards.
2. **Create a Copy**: Use the first split log entry (where `split.index` = 0) as the base log.
3. **Append Data**: For each subsequent split entry, append the contents of its fields to the corresponding fields in the base log. If a field exists in the new log entry but not in the base log, add it.
4. **Handle Repeated Fields**: If a field is a repeated list, maintain the correct element positions by joining the values across split entries.
5. **Final Touches**: Once all entries are reassembled, remove the `split` field and fix the `insertId` to remove the `.0` suffix.

### Example: Splitting and Reassembling

Let’s say a log entry contains a long string that gets split across multiple entries:

*   **Original (before splitting)**:

    ```json
    "stringField": "Very long string that needs 2 log entries."
    ```
*   **After Splitting (Entry 1)**:

    ```json
    "stringField": "Very long string that "
    ```
*   **After Splitting (Entry 2)**:

    ```json
    "stringField": "needs 2 log entries."
    ```

To reassemble:

1. Combine `stringField` values from both entries: `"Very long string that "` + `"needs 2 log entries."`
2. The result is: `"Very long string that needs 2 log entries."`

### Querying Split Logs

To find split logs:

* **Find logs from the same original entry**: Use `split.uid="UID"`.
* **Find all split logs**: Use `split:*`.
* **Filter only the first split log**: Use `split.index = 0`.
