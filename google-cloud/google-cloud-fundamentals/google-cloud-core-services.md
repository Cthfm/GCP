# Google Cloud Core Services

## Overview

Below is a breakdown of GCP's primary services, organized by category, with examples to make it easier to understand. These services are the foundation upon which most cloud architectures are built.

### 1. **Compute Services (Virtual Machines and Containers)**

These services provide the core computing power needed to run applications.

* **Compute Engine**:\
  Virtual machines (VMs) similar to AWS EC2 or Azure Virtual Machines. Example: Running a web server on a Linux VM.
* **Google Kubernetes Engine (GKE)**:\
  Managed Kubernetes service to run containers. Example: Deploying microservices inside containers across multiple nodes.
* **Cloud Run**:\
  Fully managed service to run containers without managing infrastructure. Example: Hosting a stateless API with automatic scaling.

### 2. **Storage Services**

GCP offers multiple options to store data, from files to databases.

* **Cloud Storage**:\
  Object storage for storing unstructured data like media files, backups, or logs. Example: Store videos for streaming applications.
* **Persistent Disks & Filestore**:\
  Block and file storage for VMs and applications. Example: Attach a disk to a VM for hosting a database.
* **BigQuery**:\
  Serverless data warehouse service for analyzing large datasets. Example: Running analytics queries on petabytes of e-commerce data.

### 3. **Networking Services**

These services allow you to control network traffic and interconnect services.

* **VPC (Virtual Private Cloud)**:\
  Creates isolated networks to run services privately within GCP. Example: Setting up a private network for web and database servers.
* **Cloud Load Balancing**:\
  Distributes incoming traffic across multiple backend servers. Example: Balancing traffic for a high-traffic website.
* **Cloud CDN**:\
  Content Delivery Network for delivering content globally with low latency. Example: Distributing static assets (like images) for a website.

***

### 4. **Database Services**

GCP provides managed relational and NoSQL databases.

* **Cloud SQL**:\
  Managed relational database supporting MySQL, PostgreSQL, and SQL Server. Example: Hosting a WordPress site with MySQL.
* **Cloud Spanner**:\
  Globally distributed relational database with high availability. Example: Running a transactional application that requires consistency across regions.
* **Firestore / Bigtable**:\
  Managed NoSQL databases for document-based and wide-column data. Example: Storing user profiles in Firestore for a mobile app.

***

### 5. **Identity and Security Services**

These services ensure that only authorized users and systems access your resources.

* **IAM (Identity and Access Management)**:\
  Manages who can access what services. Example: Granting developers access to deploy to GKE but not to billing.
* **Cloud Identity**:\
  Identity management for managing users and authentication. Example: Integrating Google login for employees.
* **Cloud KMS (Key Management Service)**:\
  Manages encryption keys. Example: Encrypting sensitive data stored in Cloud Storage.
* **Cloud Security Command Center**:\
  Centralized dashboard for monitoring security risks. Example: Finding misconfigurations across GCP services.

***

### 6. **Operations and Monitoring Services**

These services allow you to monitor the health and performance of your applications.

* **Cloud Logging**:\
  Collects and manages logs from GCP services and applications. Example: Viewing logs from your Compute Engine instances.
* **Cloud Monitoring (formerly Stackdriver)**:\
  Provides metrics, dashboards, and alerts. Example: Setting alerts for high CPU usage on VMs.
* **Cloud Trace & Cloud Profiler**:\
  Performance analysis tools to diagnose bottlenecks. Example: Profiling a microservice to find slow endpoints.

***

### 7. **Machine Learning & Data Analytics Services**

GCP provides tools for AI, data analytics, and insights.

* **Vertex AI**:\
  Platform to build, train, and deploy machine learning models. Example: Deploying a machine learning model for product recommendations.
* **BigQuery ML**:\
  Allows you to create ML models directly in BigQuery using SQL. Example: Predicting customer churn using transactional data.
* **Cloud Dataflow**:\
  A data pipeline service for batch and stream data. Example: Real-time log ingestion for analysis.

### 8. **Developer Tools and DevOps**

These tools help developers build, deploy, and manage applications.

* **Cloud Build**:\
  CI/CD tool to build and deploy applications. Example: Automating the deployment of code to Cloud Run.
* **Artifact Registry**:\
  Stores container images and packages. Example: Hosting Docker images for internal applications.
* **Cloud Source Repositories**:\
  Git-based repositories for code. Example: Hosting a private Git repository for a project.

### 9. **Serverless and Event-Driven Services**

These services are designed for event-driven architecture and serverless functions.

* **Cloud Functions**:\
  Event-driven functions to run code without managing servers. Example: Automatically resizing uploaded images in Cloud Storage.
* **Pub/Sub**:\
  Asynchronous messaging service for decoupling services. Example: Sending notifications between microservices.
* **Eventarc**:\
  Route events from GCP services to Cloud Functions or Cloud Run. Example: Triggering a function whenever a new file is added to Cloud Storage.

### 10. **Hybrid and Multi-Cloud Services**

GCP also supports hybrid and multi-cloud environments.

* **Anthos**:\
  Platform for managing Kubernetes clusters across hybrid and multi-cloud environments. Example: Managing workloads across GCP and on-premise.
* **Cloud VPN**:\
  Establishes secure tunnels between on-premise networks and GCP. Example: Connecting your corporate network to GCP resources.
* **Cloud Interconnect**:\
  Provides dedicated network connections between GCP and on-premise data centers. Example: Fast and secure data transfer between your data center and GCP.
