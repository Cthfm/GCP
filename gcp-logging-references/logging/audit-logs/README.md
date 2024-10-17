# Audit Logs

## What are Cloud Audit Logs?

Cloud Audit Logs track and record actions taken in your Google Cloud environment. These logs can help you understand **who did what, where, and when** within your Google Cloud project, folder, or organization. There are different types of audit logs depending on the type of activity you are interested in:

* **Admin Activity**: Records when changes are made to settings, permissions, and configurations.
* **Data Access**: Tracks data read and write operations.
* **System Event**: Captures events like system maintenance or changes made by Google Cloud's internal services.
* **Policy Denied**: Logs when a policy, such as IAM, denies an action.

### Structure of an Audit Log Entry

Each audit log entry has specific fields to help identify and interpret what happened:

1. **Project/Folder/Organization**: The entity (such as a project) that owns the log.
2. **Resource**: The object being acted upon. This could be a virtual machine (VM), a storage bucket, or another cloud resource.
3. **Timestamp**: The exact time when the action occurred.
4. **Service**: The Google Cloud product involved (e.g., Compute Engine, Cloud Storage, or Cloud SQL). It’s stored in the `protoPayload.serviceName` field.
5. **Payload**: Contains details about the action, such as who performed it and what was done. The `protoPayload` field holds an `AuditLog` object that stores this information.
6. **Log Name**: Each log entry is part of a specific log, which identifies the type of audit log it belongs to (e.g., admin activity, data access).

#### Sample Log Entry Example

Here's a simplified breakdown of a typical Admin Activity audit log entry:

```json
{
  "protoPayload": {
    "@type": "type.googleapis.com/google.cloud.audit.AuditLog",
    "authenticationInfo": {
      "principalEmail": "user@example.com"
    },
    "serviceName": "appengine.googleapis.com",
    "methodName": "SetIamPolicy",
    "request": {
      "resource": "my-gcp-project-id"
    },
    "response": {}
  },
  "timestamp": "2019-05-27T16:24:56.135Z",
  "severity": "NOTICE",
  "logName": "projects/my-gcp-project-id/logs/cloudaudit.googleapis.com%2Factivity"
}
```

* **principalEmail**: Who performed the action (`user@example.com`).
* **serviceName**: What service was used (App Engine, in this case).
* **methodName**: What action was performed (`SetIamPolicy`, meaning a change to permissions).
* **resource**: The resource affected (`my-gcp-project-id`).

### Interpreting the Entry

To interpret a log entry:

* **Is it an audit log entry?** Check if `protoPayload.@type` contains `AuditLog`.
* **What service created the log?** The `serviceName` tells you the product (e.g., `appengine.googleapis.com`).
* **What operation was audited?** `methodName` describes the specific action (`SetIamPolicy`).
* **What resource was affected?** The `resource` field specifies the Google Cloud resource (e.g., an App Engine application in project `my-gcp-project-id`).

### Key Considerations

* **Long-Running Operations**: Sometimes, an API call is ongoing, so you’ll see two logs—one when the operation starts and another when it finishes.
* **Streaming APIs**: For streaming services, two logs are created: one when the stream starts and another when it ends.

By focusing on fields like **principalEmail**, **serviceName**, and **methodName**, you can quickly see what actions were performed and who performed them, which is critical for auditing and security analysis in your cloud environment.

### Log Name Structure

The following provides the naming schema for each log type and how it is named.

```
   projects/PROJECT_ID/logs/cloudaudit.googleapis.com%2Factivity
   projects/PROJECT_ID/logs/cloudaudit.googleapis.com%2Fdata_access
   projects/PROJECT_ID/logs/cloudaudit.googleapis.com%2Fsystem_event
   projects/PROJECT_ID/logs/cloudaudit.googleapis.com%2Fpolicy

   folders/FOLDER_ID/logs/cloudaudit.googleapis.com%2Factivity
   folders/FOLDER_ID/logs/cloudaudit.googleapis.com%2Fdata_access
   folders/FOLDER_ID/logs/cloudaudit.googleapis.com%2Fsystem_event
   folders/FOLDER_ID/logs/cloudaudit.googleapis.com%2Fpolicy

   billingAccounts/BILLING_ACCOUNT_ID/logs/cloudaudit.googleapis.com%2Factivity
   billingAccounts/BILLING_ACCOUNT_ID/logs/cloudaudit.googleapis.com%2Fdata_access
   billingAccounts/BILLING_ACCOUNT_ID/logs/cloudaudit.googleapis.com%2Fsystem_event
   billingAccounts/BILLING_ACCOUNT_ID/logs/cloudaudit.googleapis.com%2Fpolicy

   organizations/ORGANIZATION_ID/logs/cloudaudit.googleapis.com%2Factivity
   organizations/ORGANIZATION_ID/logs/cloudaudit.googleapis.com%2Fdata_access
   organizations/ORGANIZATION_ID/logs/cloudaudit.googleapis.com%2Fsystem_event
   organizations/ORGANIZATION_ID/logs/cloudaudit.googleapis.com%2Fpolicy
```
