# Roles and Permissions

## Overview

In Google Cloud Platform (GCP), **roles** and **permissions** are key components of **Identity and Access Management (IAM)**, which controls who can do what on specific resources. Understanding the structure and purpose of roles and permissions is crucial for securing your cloud environment. Here's a clearer explanation:

#### 1. **Roles**:

A **role** is a collection of **permissions** that define what actions can be performed on GCP resources. These roles are granted to **principals** (such as users, groups, or service accounts) to control access to your resources.

**Three types of IAM roles:**

* **Basic roles** (also called primitive roles):
  * These existed before IAM and offer broad, coarse-grained permissions.
  * Examples:
    * **Owner**: Full control over all resources in a project.
    * **Editor**: Can modify existing resources.
    * **Viewer**: Can only view resources without modifying them.
  * **Warning**: Basic roles include too many permissions and are usually not recommended for production environments because they violate the principle of least privilege.
* **Predefined roles**:
  * These are created and maintained by Google and are specific to services (e.g., Compute Engine, Cloud Storage).
  * They provide more granular control than basic roles, such as allowing someone to create instances but not manage networks.
  * Example:
    * **Compute Admin**: Can manage Compute Engine instances but not networks.
* **Custom roles**:
  * These are user-defined and allow you to create a role with a specific set of permissions to meet your organization’s exact needs.
  * Example: You could create a role that allows only specific permissions (e.g., read access to storage buckets) that aren’t available in the predefined roles.

#### 2. **Permissions**:

Permissions define what actions can be performed on GCP resources. Each permission follows the format:\
`SERVICE.RESOURCE.VERB`

For example:

* **`compute.instances.list`**: Allows a user to list the Compute Engine instances in a project.
* **`compute.instances.stop`**: Allows a user to stop a virtual machine (VM) in Compute Engine.

Permissions are tied to specific API methods. For example, to use the Pub/Sub API method `projects.topics.publish`, you need the **`pubsub.topics.publish`** permission.

#### 3. **Role Components**:

Each role contains several components:

* **Title**: A human-readable name (e.g., "Compute Admin").
* **Name**: A unique identifier for the role. Examples:
  * Predefined role: `roles/compute.admin`
  * Project-level custom role: `projects/my-project/roles/myCustomRole`
  * Organization-level custom role: `organizations/my-org/roles/myCustomRole`
* **Permissions**: The actual permissions included in the role (e.g., `compute.instances.list`, `compute.instances.stop`).
* **Description**: A description of what the role does.
* **Stage**: The lifecycle stage of the role, like **ALPHA**, **BETA**, or **GA** (General Availability).

#### 4. **Basic Roles**:

These include the **Owner**, **Editor**, and **Viewer** roles. Basic roles are broad and should generally be avoided in favor of predefined or custom roles because they give more permissions than needed, which could expose your environment to risks.

| **Basic Role** | **Permissions**                                                                 |
| -------------- | ------------------------------------------------------------------------------- |
| **Viewer**     | Read-only actions (e.g., view resources, no changes)                            |
| **Editor**     | All viewer permissions + ability to modify resources                            |
| **Owner**      | All editor permissions + full control (e.g., manage permissions, billing setup) |

#### 5. **Predefined Roles**:

Google manages these roles to provide fine-grained access control for specific services. Predefined roles are service-specific and automatically updated when new features are added. You can assign multiple roles to the same principal at different levels (organization, project, etc.).

Example predefined roles:

* **Storage Admin**: Full control over Cloud Storage.
* **BigQuery Data Viewer**: Can view BigQuery datasets and tables but can’t modify them.

#### 6. **Custom Roles**:

Custom roles are useful when predefined roles grant too many permissions. They allow you to combine specific permissions into a role tailored to your needs.

* **Use Case**: Suppose your team needs access to read data in Cloud Storage but shouldn't have write permissions. A custom role can be created to give exactly that permission without any excess.

**Important considerations for custom roles:**

* Custom roles can be created at the **organization** or **project** level.
* You can include up to **3,000 permissions** in a custom role.
* Some permissions may only be used at the organization level and not at the project level.
* **Custom Role Limits**:
  * **300 custom roles** per organization/project.
  * Role IDs and descriptions have size limits (64 KB for permissions, 100 bytes for titles).

**Caution:**

Allow only trusted users to create and manage custom roles because they can misuse permissions if given excessive power.

#### 7. **Permission Dependencies**:

Some permissions rely on others to function properly. For example, to modify a policy, you must also have permission to read the policy. When designing custom roles, it’s important to consider these dependencies to avoid creating roles that don’t work as expected.

#### 8. **Lifecycle of Custom Roles**:

Custom roles have a lifecycle stage (e.g., **ALPHA**, **BETA**, **GA**, **DISABLED**) to track their readiness for use:

* **ALPHA/BETA**: Still in development or testing.
* **GA**: Fully tested and ready for use.
* **DISABLED**: Role is disabled but may still appear in policies (no effect).

#### Best Practices for Roles and Permissions in GCP:

* **Principle of Least Privilege**: Always grant the minimum necessary permissions. Avoid using basic roles (like **Owner** and **Editor**) in production.
* **Predefined Roles**: Use predefined roles whenever possible since they are managed by Google and automatically updated.
* **Custom Roles**: Use custom roles when predefined roles don't meet your specific needs. Always review predefined roles first before creating custom ones.
