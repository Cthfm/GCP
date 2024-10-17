# Google Cloud Hierarchy

## Overview

The Google Cloud resource hierarchy is designed to help manage resources in a structured and organized manner, similar to how files and directories are organized in a traditional operating system. This structure has two main purposes: to manage the ownership lifecycle of resources and to establish a system for access control and policy inheritance.

### Main Purposes of the Resource Hierarchy:

1. **Ownership Binding**: Each resource in Google Cloud is bound to a specific parent resource, which determines the resource's lifecycle. This means that if a parent resource is deleted, its child resources are deleted as well. This ensures a clear and organized relationship between resources.
2. **Access Control and Policy Inheritance**: The hierarchy also provides attachment points for access control and organization policies. Identity and Access Management (IAM) settings, as well as organization policies, applied at higher levels in the hierarchy are automatically inherited by lower-level resources. This allows organizations to efficiently manage permissions and policies across multiple resources.

### Hierarchical Structure:

The Google Cloud resource hierarchy consists of four key levels: **organization**, **folder**, **project**, and **service resources**. Each resource except for the top-level organization resource has a single parent, and each layer inherits permissions and policies from its parent.



<figure><img src="../../.gitbook/assets/cloud-hierarchy.svg" alt=""><figcaption><p><a href="https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy">https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy</a></p></figcaption></figure>



**1. Organization Resource:**

* **Top of the hierarchy**: The organization resource represents the entire company or domain. It is the root node when it exists, and all other resources within the company fall under this root node.
* **Link to Google Workspace/Cloud Identity**: An organization resource is closely tied to Google Workspace or Cloud Identity accounts. Each company or domain can have exactly one organization resource. When a user from a Google Workspace or Cloud Identity account creates a project, an organization resource is automatically provisioned if it does not already exist.
* **Centralized Management**: With an organization resource, administrators have central control over all resources in the hierarchy. They can view, manage, and set IAM policies at the organization level, which are then inherited by all child resources like folders and projects.
* **Benefits**: This setup ensures that resources belong to the organization rather than an individual user. For example, projects created by an employee are linked to the organization, so if that employee leaves, the projects remain intact and under the organization's control.

**2. Folder Resource:**

* **Optional grouping layer**: Folder resources are optional and allow for further structuring within the organization. Folders can be used to represent departments, legal entities, teams, or any other logical group within the organization.
* **Policy Inheritance**: Like the organization resource, folders act as policy inheritance points. IAM roles and organization policies set on a folder are inherited by all project and service resources under that folder.
* **Use Cases**: Folders provide a way to delegate administration rights. For example, each department head can be given ownership of all Google Cloud resources in their department, and different departments can have different IAM policies applied based on their folders.

**3. Project Resource:**

* **Fundamental organizing entity**: Projects are the core organizational entities in Google Cloud. Every Google Cloud service and resource (e.g., Compute Engine virtual machines, Pub/Sub topics, Cloud Storage buckets) exists within a project.
* **Mandatory for all resources**: A project resource is required to use any Google Cloud service. Projects manage the billing, API usage, permissions, and all other settings associated with the resources they contain.
* **Identifiers**: Each project has a unique project ID and project number, which are used to identify and interact with resources in Google Cloud.
* **Policy Inheritance**: IAM policies applied at the project level are inherited by the child resources (such as VMs or buckets) within the project. For instance, if a user is granted "Editor" access to a project, they have that access to all resources under that project unless overridden at a lower level.

**4. Service Resources:**

* **Lowest level in the hierarchy**: Service resources are the individual resources that make up Google Cloud services, such as virtual machines (VMs), databases, storage buckets, and Pub/Sub topics. These resources are owned by a project, and their lifecycle is tied to the project’s lifecycle.
* **Child Resources**: Service resources inherit policies from their parent project, so managing access at the project level is often sufficient for controlling access to these resources.

### Policy Inheritance:

One of the most powerful features of the Google Cloud resource hierarchy is its inheritance model for IAM roles and organization policies. Policies and permissions applied at a higher level (such as an organization or folder) automatically flow down to lower levels (projects and service resources).

* **IAM Policies**: These policies define who has access to what resources and what actions they are allowed to perform. IAM policies can be set at any level—organization, folder, project, or even individual resources—and are inherited by lower levels. For example, if you assign a role to a user at the folder level, that user will have that role for all projects and resources within that folder.
* **Effective Policy**: The effective policy for a resource is the combination of the policy applied directly to that resource and the policies inherited from its parent resources. If a user is granted access at a higher level, that access cannot be revoked by a lower-level resource.
* **Transitive Inheritance**: Policies are inherited transversely, meaning permissions set at the top (organization level) are passed down to folders, then projects, and finally individual resources like VMs and buckets.

### Examples of Policy Inheritance:

* If an administrator sets a policy at the **organization level**, granting the "Network Admin" role to a user, that user will have network management rights across all projects and resources within the organization.
* If a policy is applied to a **folder**, all projects and resources under that folder inherit the policy. For instance, granting a user the "Project Editor" role at the folder level means that the user will have editor rights for all projects and resources under that folder.
* Conversely, if a user is assigned a role at the **project level**, their permissions will only apply to the specific project and its resources, not to any other projects or resources in other folders or the organization.

### Benefits of the Hierarchy:

1. **Centralized Control and Visibility**: Administrators have full visibility and control over all resources within the organization. This reduces the risk of "shadow projects" or unauthorized resources being created.
2. **Policy Management**: Access control can be managed centrally at the organization level, with policies automatically inheriting down the hierarchy, simplifying the management of large, complex environments.
3. **Resource Lifecycle Management**: Resources are tied to the organization or folders, not individuals. This ensures continuity and prevents data or project loss when employees leave the company.
