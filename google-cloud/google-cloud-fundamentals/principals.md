# Principals

## Overview

In Google Cloud Platform (GCP), several principal types are used for Identity and Access Management (IAM) purposes. These principal types represent the entities that can interact with GCP resources, and they are central to granting roles and permissions. The main principal types in GCP include:

1. **Google Accounts (User Accounts):**
   * Individual users who have a Google account (either a Gmail account or a Google Workspace account).
   * Typically used to represent human users who need access to GCP resources.
2. **Service Accounts:**
   * Special types of Google accounts intended to represent non-human users, such as applications or services, that need to interact with GCP resources.
   * Used extensively in automation and resource-to-resource interactions.
3. **Google Groups:**
   * A collection of Google accounts or service accounts that can be granted permissions as a group rather than individually.
   * Useful for managing permissions for multiple users or service accounts at once.
4. **Cloud Identity or Google Workspace Domains:**
   * Organizations using Google Workspace or Cloud Identity can apply roles and permissions to the entire domain or specific organizational units within that domain.
   * Useful for managing access across a larger organization.
5. **GCP Projects:**
   * Projects themselves can be considered a principal when sharing resources across different projects.
   * Useful for cross-project access control.
6. **App Engine Applications:**
   * App Engine apps are considered principals when granting or restricting their access to other resources within GCP.

Each of these principal types can be assigned roles to grant specific permissions to resources, providing a flexible and scalable way to manage access across GCP environments.
