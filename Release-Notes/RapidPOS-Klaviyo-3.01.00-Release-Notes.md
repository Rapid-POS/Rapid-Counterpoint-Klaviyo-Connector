# Rapid POS Klaviyo Connector v3.01.00 Release Notes 
 
_Release Date: March 11, 2025_  
 
---
 
## New Functionality 
 
### Klaviyo API Revision Update and Breaking Change Handling 
 
- Updated the connector to support the latest stable Klaviyo API revision (**2025-01-15**). 
  - This revision introduces a breaking change that now requires the **subscription field** to be included in unsubscribe requests. 
  - The connector has been updated to ensure unsubscribe requests include the required subscription data. 
 
### Dynamic Klaviyo API Revision Configuration 
 
- Removed the hard-coded Klaviyo API revision from the source code. 
  - The connector now dynamically reads the API revision from `USER_KLAVIYO_CONFIG.API_REVISION`. 
 
### Enhanced Customer Down Processing Using Calculated Queries 
 
- Enhanced **Customer Down** logic to support calculated fields. 
  - The connector now utilizes the `CALCULATED_COLUMN_QUERY` during the customer down process. 
 
### Profile Validation Before Ticket Sync 
 
- Added validation to ensure a Klaviyo customer profile exists before inserting tickets. 
  - Before a ticket is sent to Klaviyo, the connector checks whether the corresponding customer profile already exists. 
  - If the profile exists, the ticket is inserted as normal. 
  - If the profile does not exist, the system prompts to insert the customer profile first before proceeding with the ticket insertion. 
  - This ensures all tickets in Klaviyo are properly associated with valid customer profiles. 
 
---
 
## Schema Changes 
 
### Auto-Enroll Condition Update 
 
- Updated the SQL logic for Auto-Enroll behavior. 
  - The auto-enroll condition was changed from `AUTO_ENROLL` = `Y` to `AUTO_ENROLL` <> `N`. 
 
### Status Update Logic Adjustment in `AR_CUST` 
 
- Returned the logic for setting the Klaviyo status to `1` back to the `AR_CUST` update trigger. 
  - Ensures consistent status updates when customer records are modified in Counterpoint. 
