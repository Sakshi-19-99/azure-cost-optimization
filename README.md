## Overview
This solution addresses the growing cost of storing billing records in Azure Cosmos DB by archiving records
older than 90 days into Azure Blob Storage. It ensures cost optimization without changing the existing
APIs and keeps archived data accessible within a few seconds.

## Components
- **Azure Cosmos DB**: Stores active billing records (< 90 days)
- **Azure Blob Storage**: Stores archived records (> 90 days)
- **Azure Functions**:
  - `archiveBillingRecords`: Timer-triggered archival function
  - `readBillingRecord`: HTTP-triggered proxy
- **Terraform**: Used for infrastructure provisioning

## How It Works
1. **Archival Job (`archiveBillingRecords`)**
   - Runs daily via Timer Trigger
   - Moves records older than 90 days to Blob Storage
   - Deletes migrated records from Cosmos DB

2. **Read Proxy (`readBillingRecord`)**
   - Receives `GET` requests
   - Tries Cosmos DB first
   - Falls back to Blob Storage if not found

3. **No API Changes**
   - The same endpoint supports both active and archived records

## Edge Cases & Reliability
- Records not found in Cosmos DB are served from Blob
- Logging can track record access and migration status
- Retry logic can be implemented for robustness
- Monitoring via Azure Monitor and Application Insights


**Architecture Diagram**


![image](https://github.com/user-attachments/assets/ec416d11-6dac-4f56-8c9d-249aaed984e8)

**Advantages**
  • Completely serverless and scalable.
  • Cuts costs by archiving infrequently accessed data.
  • No downtime or data loss.
  • Integrates with current APIs seamlessly.
 
