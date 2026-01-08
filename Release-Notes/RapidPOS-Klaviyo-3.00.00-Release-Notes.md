# Rapid POS Klaviyo Connector – Release Notes 
 
## Version 3.0.0  
**Release Date:** December 9, 2024  
 
---
 
## Major Changes 
 
### CI/CD Compatibility 
 
- The Klaviyo connector is now fully **CI/CD compatible**. 
  - CI/CD stands for Continuous Integration/Continuous Deployment. Previously, when a new feature was developed or a bug was fixed, we would have to manually perform a re-install of that connector at every client. CI/CD means the fixes and new features are automatically deployed, sometimes fixing a bug before you ever experience it. 
 
### Migration from Task Scheduler to Windows Service 
 
- Migrated connector execution from **Windows Task Scheduler** to a **Windows Service**. 
  - Improves reliability and long-term stability. 
  - Enhanced scheduling flexibility: 
    - Separate schedules can now be configured for **profile processing** and **ticket processing**. 
    - Ticket processing can be prioritized to run before profile runtime. 

---
 
## New Functionality 
 
### Klaviyo Customer Table and Subscription Management 
 
- Introduced a new Klaviyo customer table and supporting functionality. 
 
- Added a new form for **subscription and opt-in management**. 
  - Allows monitoring of customer sync status. 
  - Provides visibility into customer subscription state. 
 
- Enhanced unsubscribe tracking. 
  - When a customer unsubscribes, it is now possible to determine whether the customer has recently unsubscribed or has been suppressed. 
 
### Separated Customer Mapping Controls 
 
- Added two new customer mapping buttons. 
  - **Customer Up** and **Customer Down** mappings are now separated into distinct buttons. 
  - Replaces the previous single combined mapping button. 
  - Improves clarity and control over customer synchronization direction. 
 
### Klaviyo Custom Events 
 
- Added support for **Custom Klaviyo Events**. 
  - Implemented a custom data query framework to generate event data from Counterpoint. 
  - Enables syncing of information that cannot be handled through standard mapping tables. 
  - Supports advanced use cases beyond orders and tickets, such as delivery or shipping-related data. 
 
### Documents Up Configuration Enhancements 
 
- Expanded the data that can be sent with documents synced to Klaviyo. 
  - In addition to standard document data, clients can now send additional information such as item-level details. 
  - Supports up to **10 nested objects**: 
    - The first **2 nested objects** are reserved for document-level data (order line and payment). 
    - Up to **8 additional nested objects** can be configured by the client. 
 
- Clarified property hierarchy: 
  - **Top-level properties**: document lines and document payments 
  - **Detail-level properties**: fields within each document line or payment 
  - Configuration is managed through the **Klaviyo Custom Properties Documents Up** table. 
 
### Klaviyo Customer Add-on-the-Fly 
 
- Added a new form to allow cashiers to manually add Klaviyo customers during checkout. 
  - Supports entry of: 
    - Customer name 
    - Email address 
    - SMS number 
    - Klaviyo profile ID 
  - Includes an option to always send an email receipt via Klaviyo. 
 
### Queue Management for Klaviyo Uploads 
 
- Added enhanced queue tracking for documents and custom events being sent to Klaviyo. 
  - Each transaction is assigned a unique **queue ID**. 
  - Allows users to monitor the status of each document or event upload. 
 
- Added a new button for viewing queue details. 
  - Displays information such as: 
    - Account name 
    - Queue ID 
    - Customer details 
    - Synchronization status 
  - Improves visibility and troubleshooting of Klaviyo uploads. 
