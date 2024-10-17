# IAM Fundamentals

## **Overview**

Identity and Access Management (IAM) in Google Cloud Platform (GCP) is a system that helps manage who (identities) has what access (roles) to which resources. It ensures that only the right people or services have the right level of access to perform specific actions, following the principle of least privilege.

### **Key Components of GCP IAM**

#### 1. **Identities (Who can access resources)**

An **identity** is any entity that interacts with GCP resources. It can be a person, application, or service.

* **Google Accounts**:\
  Personal Google accounts like `alice@gmail.com`.
* **Cloud Identity Users**:\
  Employees or users managed by your organization, e.g., `bob@yourcompany.com`.
* **Groups**:\
  A collection of users, e.g., `developers-team@yourcompany.com`. You can assign roles to the entire group to manage access efficiently.
* **Service Accounts**:\
  **Machine identities** used by applications, services, or virtual machines to interact with GCP resources.\
  **Example**: A virtual machine accesses a Cloud Storage bucket using a service account.

#### 2. **Resources (What needs to be protected)**

GCP provides various **resources** you need to control access to. Some examples include:

* **Compute Engine VMs** – Virtual machines that run workloads.
* **Cloud Storage Buckets** – Used to store files or backups.
* **BigQuery Datasets** – Large datasets for analytics.
* **Cloud SQL Instances** – Managed relational databases.

When assigning permissions, you specify which **resource** a role applies to. For example, you could give a user read-only access to a specific storage bucket.

#### 3. **Roles (What actions can be performed)**

A **role** is a collection of **permissions** that define what actions can be performed on resources. In GCP, there are three types of roles:

1.  **Primitive Roles** (Legacy Roles):\
    These are broad roles that apply across a project:

    * `Owner`: Full access to all resources.
    * `Editor`: Can modify resources.
    * `Viewer`: Read-only access.

    **Note**: Avoid using these roles as they provide more access than typically needed.
2. **Predefined Roles**:\
   These roles are specific to a service and include only the permissions needed for common tasks.\
   **Example**: `Storage Object Viewer` allows read-only access to objects in Cloud Storage, but not the ability to delete them.
3. **Custom Roles**:\
   You can create your own roles with specific permissions based on your organization’s needs.

#### 4. **IAM Policies (How permissions are assigned)**

IAM **policies** define which identities have what roles on which resources.\
Each policy contains **bindings** that specify **roles** and the **members (identities)** assigned to those roles.

**Example Policy**:

```json
{
  "bindings": [
    {
      "role": "roles/storage.objectViewer",
      "members": [
        "user:alice@gmail.com"
      ]
    }
  ]
}
```

In this example, `alice@gmail.com` is given the `Storage Object Viewer` role, which allows her to read objects in a specific storage bucket.

### **Best Practices for Using IAM in GCP**

1. **Principle of Least Privilege**
   * Only grant users or service accounts the **minimal permissions** required for their tasks.
   * Example: If someone only needs to view logs, assign them the `Log Viewer` role instead of the `Editor` role.
2. **Use Groups for Permissions**
   * Instead of assigning roles to individual users, assign them to **groups**. This makes managing permissions easier.
   * Example: Add all developers to a `developers-team@yourcompany.com` group and assign roles to the group rather than each individual.
3. **Use Service Accounts for Applications**
   * Avoid using personal accounts for automated processes. Use **service accounts** instead.
   * Example: A Compute Engine VM accessing a database should use a service account with only the necessary permissions.
4. **Audit IAM Policies Regularly**
   * Periodically review policies to ensure that no unnecessary permissions have been granted.
5. **Monitor Role Changes**
   * Keep track of who has been assigned which roles. This helps prevent accidental or unauthorized access.
6. **Avoid Long-term Service Account Keys**
   * Use **Workload Identity Federation** or rotate service account keys regularly to reduce the risk of key compromises.

### **Commands with gcloud CLI for Managing IAM**

Here are some useful `gcloud` commands for managing IAM roles and policies:

1.  **List all IAM roles and policies in a project**:

    ```bash
    gcloud projects get-iam-policy my-project
    ```
2.  **List all service accounts in a project**:

    ```bash
    gcloud iam service-accounts list --project=my-project
    ```
3.  **Check for users with high-level roles (Owner or Editor)**:

    ```bash
    gcloud projects get-iam-policy my-project --filter="bindings.role:roles/owner OR bindings.role:roles/editor"
    ```

These commands help you view the current IAM setup and ensure that the correct roles are assigned to identities.
