# Short Architecture Walkthrough: Compute Hosting + AI Service

## Scenario

A company wants a customer support web application where users can submit questions and receive AI-assisted responses. The solution must be simple to manage, scalable, and suitable for a cloud-first design.

## Architecture Overview

```text
Users
  -> Azure App Service (web application)
  -> Azure AI Language / Azure AI service
  -> Azure SQL Database
  -> Azure Storage Account
  -> Azure Monitor
```

## Walkthrough

### 1. Azure App Service

**Role:** Hosts the customer-facing web application and API.

**Why it was chosen:**

- It is a managed platform for web apps.
- It reduces infrastructure management compared to virtual machines.
- It supports autoscaling and easy deployments.
- It is a strong fit for HTTP-based applications.

This makes App Service a practical compute choice for a support portal that needs reliability without high operational overhead.

### 2. Azure AI Language / Azure AI Service

**Role:** Analyzes user questions and helps generate useful responses, such as intent detection, text analysis, or AI-assisted reply suggestions.

**Why it was chosen:**

- It adds AI capability without building and training a custom model from scratch.
- It is easier and faster to adopt than creating a full machine learning platform.
- It fits common customer-service scenarios like text understanding and summarization.

This service adds intelligent behavior to the app while keeping the architecture relatively simple.

### 3. Azure SQL Database

**Role:** Stores user profiles, support tickets, and structured application data.

**Why it was chosen:**

- It is a managed relational database.
- It provides backup, patching, and high availability features.
- It works well for transactional business data.

This is suitable for structured records such as ticket status, timestamps, and customer information.

### 4. Azure Storage Account

**Role:** Stores uploaded documents, chat attachments, and exported reports.

**Why it was chosen:**

- It is cost-effective for unstructured data.
- It integrates easily with web applications.
- It supports durable cloud storage for files and objects.

This separates file storage from the application and database tiers.

### 5. Azure Monitor

**Role:** Collects metrics, logs, and alerts for the application.

**Why it was chosen:**

- It helps track performance and failures.
- It supports alerting when the app slows down or errors increase.
- It improves operational visibility.

Monitoring is necessary because even simple cloud architectures need observability.

## End-to-End Flow

1. A user opens the support website hosted on Azure App Service.
2. The user submits a question.
3. The application sends the text to Azure AI Language or another Azure AI service for analysis.
4. The app stores ticket details in Azure SQL Database.
5. Any uploaded files are stored in Azure Storage.
6. Azure Monitor tracks response times, failures, and service health.

## Why This Architecture Works

- **App Service** was chosen for managed web hosting.
- **Azure AI Language / Azure AI service** was chosen to add intelligent text processing quickly.
- **Azure SQL Database** was chosen for structured transactional data.
- **Azure Storage** was chosen for low-cost file storage.
- **Azure Monitor** was chosen for visibility and alerting.

The result is a simple cloud architecture that combines compute hosting with AI capabilities in a way that is realistic, scalable, and easy to explain in AZ-900 terms.
