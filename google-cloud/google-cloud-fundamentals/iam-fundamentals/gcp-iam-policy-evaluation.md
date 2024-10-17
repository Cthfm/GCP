# GCP IAM Policy Evaluation

## Overview

In **Google Cloud Platform (GCP)**, **IAM policies** control access to resources by binding **roles** to **identities**. Understanding how these policies are **evaluated** is key to effectively managing access and ensuring the right permissions are granted. Below is a detailed breakdown of how GCP evaluates IAM policies:

### **How IAM Policy Evaluation Works**

When an identity (like a user or service account) tries to access a resource, GCP follows a **multi-step process** to determine whether to **allow or deny** the request. IAM uses a **default-deny** model—meaning access is **denied by default unless explicitly allowed** by a policy.

Here’s how GCP evaluates policies:

### **Step-by-Step Policy Evaluation Process**

1.  **Identify the Resource and IAM Policy Scope**

    * Every request is associated with a **specific resource** (like a VM or storage bucket).
    * GCP checks the **IAM policies** associated with that resource and its **parent resources** (like projects or folders).
    * **Policy inheritance**: Policies at **higher levels (folder, project)** apply to resources inside them, unless overridden.

    **Example**: A storage bucket inherits the IAM policy from the project it belongs to, but it can also have its own bucket-level policy.
2.  **Check for Relevant Policies**

    * IAM policies contain **bindings** that assign **roles** to **members** (identities).
    * GCP checks if the identity making the request is listed in the policy or inherits a role from higher-level resources.

    **Example Binding**:

    ```json
    {
      "role": "roles/storage.objectViewer",
      "members": [
        "user:alice@gmail.com"
      ]
    }
    ```

    If the identity (e.g., `alice@gmail.com`) matches, GCP continues evaluating the request based on the role.
3.  **Match the Role’s Permissions to the Request**

    * GCP determines whether the requested **action** is allowed by the **permissions** associated with the role.
    * Roles are collections of permissions. For example:
      * `roles/storage.objectViewer` includes the `storage.objects.get` permission.

    **Example**: If Alice tries to **download an object**, GCP checks if `storage.objects.get` is included in the `roles/storage.objectViewer` role assigned to her.
4.  **Policy Evaluation Order (Hierarchical Check)**\
    Policies are **evaluated in order from the resource level upwards**, following this hierarchy:

    * **Resource-level policy** (e.g., specific bucket or VM)
    * **Project-level policy**
    * **Folder-level policy** (if applicable)
    * **Organization-level policy** (if applicable)

    If a **role is granted** at any level in the hierarchy, the request is allowed unless explicitly denied.
5.  **Check for Deny Policies (Explicit Denials)**\
    GCP has **Deny policies**, which are evaluated separately from allow policies. Deny policies take **precedence**—if a deny policy matches, the request is **immediately denied**, even if other policies allow it.

    **Deny Policy Example**:

    ```json
    {
      "denyRules": [
        {
          "deniedPrincipals": ["user:bob@gmail.com"],
          "deniedPermissions": ["storage.objects.get"]
        }
      ]
    }
    ```

    If Bob is included in this policy, his request to download an object will be denied, no matter what other policies say.
6. **Default Deny (Implicit Denial)**
   * If no policy explicitly **allows** the requested action, the request is **denied by default**.
   * This ensures that **only explicitly granted permissions** allow access, following the principle of least privilege.

### **Policy Evaluation Flowchart Summary**

1. **Is the identity allowed by a role binding in a policy?**
   * If **yes**, move to step 2. If **no**, deny the request.
2. **Does the role contain the required permission for the requested action?**
   * If **yes**, continue. If **no**, deny the request.
3. **Is there a deny policy that explicitly blocks the action?**
   * If **yes**, deny the request. If **no**, continue.
4. **Is access explicitly allowed at any resource level or inherited?**
   * If **yes**, allow the request. If **no**, deny the request by default.

### **Examples of Policy Inheritance and Evaluation**

#### Example 1: **Storage Bucket Inheritance**

* **Project-level policy**:\
  Alice has the `roles/viewer` role at the **project level**.
* **Bucket-level policy**:\
  The bucket has an IAM policy granting the `roles/storage.objectViewer` role to Bob.
* **Alice’s request**:\
  She can **view project resources**, but she **cannot download files** from the bucket because she wasn’t granted the storage-specific role.

#### Example 2: **Explicit Deny Overrides Allow**

* **Folder-level policy**:\
  Bob has the `roles/storage.admin` role, which gives full access to storage buckets.
* **Deny policy on a specific bucket**:\
  A policy explicitly denies Bob the `storage.objects.get` permission.
* **Bob’s request**:\
  Even though Bob is an admin, his request to download an object is **denied** because the explicit **deny policy takes precedence**.

### **Best Practices for Managing Policies in GCP**

1. **Apply the Principle of Least Privilege**
   * Assign only the necessary permissions to users and service accounts.
2. **Use Resource-level Policies Where Possible**
   * Apply policies at the **resource level** (like individual buckets) to avoid giving unnecessary access at higher levels.
3. **Monitor Policy Changes**
   * Regularly review IAM policies for unwanted role assignments or changes.
4. **Use Deny Policies Cautiously**
   * Deny policies can block access in unexpected ways—apply them only when necessary to enforce strict controls.
