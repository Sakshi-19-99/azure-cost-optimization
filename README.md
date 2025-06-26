**Azure Serverless Cost Optimization: Billing Records Archival**
**Overview**
 This solution addresses the growing cost of storing billing records in Azure Cosmos DB by archiving records
 older than 90 days into Azure Blob Storage. It ensures cost optimization without changing the existing
 APIs and keeps archived data accessible within a few seconds

**Components**
 • Azure Cosmos DB: Stores active billing records (< 90 days)
 • Azure Blob Storage: Stores archived records (> 90 days) using Cool/Archive tier
 • Azure Functions:
   • archiveBillingRecords : Timer-triggered archival function
   • readBillingRecord : HTTP-triggered proxy that reads from Cosmos DB or Blob
 • Terraform: Used for provisioning Azure infrastructure

**How It Works**
1. Archival Job (archiveBillingRecords):
    • Runs daily via Timer Trigger
    • Moves records older than 90 days from Cosmos DB to Blob Storage
    • Deletes them from Cosmos DB after successful migration
2. Read Proxy (readBillingRecord ):
    • Receives GET requests for records
    • Checks Cosmos DB first
    • If not found, falls back to Blob Storage
3. No API changes:
    • The same endpoint is used to fetch both active and archived record

**Folder Structure**
azure-cost-optimization-serverless/
 ├── architecture/
 │   └── architecture-diagram.png
 ├── functions/
 │   ├── archiveBillingRecords/
 │   │   ├── __init__.py
 │   │   └── function.json
 │   ├── readBillingRecord/
 │   │   ├── __init__.py
 │   │   └── function.json
 ├── terraform/
 │   └── main.tf
 ├── scripts/
 │   └── test_data_loader.py
 └── README.md

**Edge Cases & Reliability**
  • Records not found in Cosmos DB are served from Blob
  • Logging can be added to track record access and migration status
  • Archival retry logic can be enhanced for production
  • Monitoring can be added using Azure Monitor + Application Insights
  • Records not found in Cosmos DB are served from Blob

**Architecture Diagram**

**Advantages**
  • Completely serverless and scalable.
  • Cuts costs by archiving infrequently accessed data.
  • No downtime or data loss.
  • Integrates with current APIs seamlessly.

