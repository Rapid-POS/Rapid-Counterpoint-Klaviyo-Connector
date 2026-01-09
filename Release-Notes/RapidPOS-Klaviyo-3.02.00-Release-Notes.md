# Rapid POS Klaviyo Connector v3.02.00 Release Notes 
 
_Release Date: June 9, 2025_  
 
---
 
## New Functionality 
 
### Klaviyo Customer Status View 
 
- Added a new tool for viewing Klaviyo customer synchronization status. 
  - Displays how many customers have a Klaviyo Customer Record and how many are currently in each status: **0, 1, 2, 5, 6, and 9**. 
  - Available from **Connector > Klaviyo** menu. 
  - Provides a form-based view showing the count of customers in each sync status for quick reference. 
  - Users can edit the column designer to view all available columns and save the layout as the default view. 
 
### Mobile Phone Number Validation for SMS 
 
- Added validation for mobile phone numbers to reduce **invalid SMS number** errors. 
  - Only mobile phone numbers containing **exactly 10 numeric digits** are now sent to Klaviyo as SMS numbers. 
  - Phone numbers with fewer than 10 or more than 10 digits will no longer be pushed to Klaviyo as mobile/SMS numbers. 
 
### Automatic Provisioning and Rebuilds During Upgrades 
 
- Added automatic database provisioning and rebuilds during upgrades for clients using offline stations in Counterpoint. 
  - Provisioning and rebuild processes are now scheduled to run automatically during approximate off-hours. 
  - Off-hours timing is determined based on ticket history to minimize operational impact. 
 
### Rollback Support for Stuck Klaviyo Customers 
 
- Implemented a rollback feature to handle Klaviyo customer records that become stuck in status `2`. 
  - This can occur when the Klaviyo Connector Service terminates unexpectedly due to events such as Windows updates or server restarts. 
  - The rollback process updates affected customer records so they can be reprocessed and synced correctly. 
 
---
 
## Bug Fixes and Performance Enhancements 

- Fixed an issue where customers with a single quote (`'`) in their name could not be downloaded when the connector was configured to import customers from Klaviyo (**Customers Down**). 
 
- Corrected validation logic when the Counterpoint customer name type is **Person** (`AR_CUST.CUST_NAM_TYP` = `P`) but the **Name** field (`AR_CUST.NAM`) is used in **Klaviyo Field Mapping Customers Down**. 
 
- Fixed a configuration issue with **Max Number of Customers to Sync** when multiple Klaviyo accounts are configured. 
  - Previously, only the value for the first account was respected. 
  - The connector now correctly applies the configured limit separately for each Klaviyo account. 
 
- Updated date handling in customer field mappings to use the **ISO 8601 DateTime format**. 
  - This enables accurate date-based filtering when building segments in Klaviyo. 
 
- Implemented separate database connections for transaction creation and query execution to reduce and prevent deadlock scenarios. 
 
---
 
## Schema Changes 
 
- Modified the Klaviyo customer table update trigger to improve overall efficiency. 
 
- Updated the data cleansing and validation function used for Klaviyo mobile/SMS numbers. 
 
- Added a new database view to support the **Klaviyo Customer Status View**. 
