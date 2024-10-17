# Account Creation Instructions

#### Step 1: Create a Google Account

If you don’t already have a Google account, you’ll need one to access GCP.

1. Visit Google Account.
2. Fill out the form with your personal information (name, email, password, etc.).
3. Click **Next** and follow the prompts to verify your email or phone number.

#### Step 2: Access Google Cloud Console

1. Once your Google account is created (or if you already have one), navigate to [Google Cloud Console](https://cloud.google.com/).
2. Click **Get started for free** or **Go to Console**.

#### Step 3: Sign Up for Free Tier

1. On the Google Cloud Console homepage, click **Start free** to sign up for the free tier.
2. You will be prompted to provide:
   * **Country**: Select your country of residence.
   * **Terms of Service**: Accept the terms and conditions by checking the box.
3. Click **Continue**.

#### Step 4: Provide Billing Information

1. Enter your **billing information**, even though you will not be charged immediately. GCP requires this for identity verification.
   * **Name and address**: Fill out your personal or business details.
   * **Credit/debit card**: Enter valid credit or debit card information. Google uses this only for identity verification. You will not be charged for free tier usage.
2. Click **Start my free trial**. You will receive a $300 credit for 90 days to explore GCP.

#### Step 5: Set Up Your First Project

1. After creating your GCP account, you will be prompted to **create a project**.
2. Click **Select a project** at the top left of the Cloud Console.
3. Click **New Project**.
4. Enter your project details:
   * **Project name**: Give your project a descriptive name (e.g., “My First GCP Project”).
   * **Location**: Choose an organization if applicable, or leave it set to **No organization**.
5. Click **Create**.

#### Step 6: Enable Billing for the Project

1. Navigate to the **Billing** section from the Cloud Console left-hand menu.
2. Select **Billing Accounts**.
3. Choose your billing account and link it to your project (you can use the same free trial credit).

#### Step 7: Activate APIs and Services

1. In the **APIs & Services** section, click **Library** from the left menu.
2. Here, you can activate any APIs you need for your project (e.g., Compute Engine, Cloud Storage, etc.).
   * Search for the API you want to enable.
   * Click **Enable** on the API overview page.

#### Step 8: Access GCP Resources

You can now begin using GCP services. To create virtual machines, databases, or other resources, use the **navigation menu** (three horizontal bars in the top left of the Cloud Console).

* **Compute Engine** for virtual machines.
* **Cloud Storage** for storing files and objects.
* **Cloud Functions** for serverless functions.

#### Step 9: Manage Permissions with IAM

1. Go to **IAM & Admin** from the Cloud Console.
2. Click on **IAM** to manage roles and permissions for your project.
3. Add users or service accounts to your project and assign them appropriate roles.

#### Step 10: Monitoring and Budget Setup

1. Set up **budget alerts** by going to the **Billing** section and selecting **Budgets & Alerts**. You can create a budget to control how much you spend.
2. Use **Google Cloud Monitoring** (from the **Operations** section) to monitor your project resources, logs, and metrics.

#### Step 11: Explore the GCP Console

1. Use the **gcloud CLI** to manage your project resources programmatically. Install the SDK from Google Cloud SDK.
2. Try exploring more GCP features such as **BigQuery**, **AI/ML services**, and **Kubernetes Engine** to broaden your cloud skills.
