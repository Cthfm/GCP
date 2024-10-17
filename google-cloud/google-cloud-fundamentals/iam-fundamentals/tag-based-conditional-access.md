# Tag-Based Conditional Access

## Overview

IAM (Identity and Access Management) Conditions with tags in Google Cloud allow for fine-grained access control. You can attach **tags**—key-value pairs—to resources like projects or virtual machines (VMs). Conditional policies can then be applied to restrict access based on these tags. For example, you can grant the **viewer role** only to resources tagged with `environment:production`. This dynamic approach simplifies permissions management by aligning access with context, such as environment, department, or compliance needs. The permissions and policies are inherited across the Google Cloud hierarchy, ensuring consistency across all levels (organization, folder, and project).

***

### Tutorial: Using Tags with IAM Conditions

**Step 1: Create a Tag Key and Value**

First, create a **tag key** (like `environment`) and assign a value (`production`) to it using the `gcloud` CLI.

```bash
gcloud resource-manager tags keys create environment \
  --parent=organizations/1234567890

gcloud resource-manager tags values create production \
  --parent=organizations/1234567890/environment
```

**Step 2: Attach the Tag to a Resource**

Use the following command to attach the `environment:production` tag to a specific project.

```bash
gcloud resource-manager tags bindings create \
  --tag-value=tagValues/1110654321 \
  --parent=//cloudresourcemanager.googleapis.com/projects/987654321
```

**Step 3: Create a Policy with a Condition Based on the Tag**

Next, modify the IAM policy for the project to grant access based on the tag condition.

Create a **policy.json** file with the following content:

```json
code{
  "bindings": [
    {
      "role": "roles/viewer",
      "members": ["user:example@example.com"],
      "condition": {
        "title": "ProductionAccess",
        "expression": "resource.tag(environment) == 'production'"
      }
    }
  ]
}
```

Apply the policy using the following command:

```bash
gcloud projects set-iam-policy 987654321 policy.json
```

**Step 4: Verify the Policy**

To ensure that the policy has been applied correctly, list the IAM policies for the project:

```bash
gcloud projects get-iam-policy 987654321
```
