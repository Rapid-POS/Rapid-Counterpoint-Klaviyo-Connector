# Rapid POS Klaviyo Connector v2.00.02 Release Notes 
 
_Release Date: July 9, 2024_  
 
---
 
## Bug Fixes and Performance Enhancements 
 
- Updated the `USER_VI_PS_DOC_HDR_KLAVIYO` view to include additional customer and store information. 
  
  - Added the **Loyalty Card Number** (`AR_CUST.LOY_CARD_NO`) and **Store Name** fields to the document header data sent to Klaviyo. 
  - Updated payload stored procedures to retrieve the store name from `USER_VI_PS_DOC_HDR_KLAVIYO`. 
 
- Fixed an error related to JSON modification in payload processing. 
  
  - Resolved the error: *“The argument 2 of the JSON_MODIFY must be a string literal.”* 
  - Updated payload stored procedures to use `sp_executesql`, addressing a SQL Server 2016–specific issue. 
 
- Fixed errors encountered when processing tickets from offline stations. 
  
  - Updated `AR_CUST` account name triggers to remove `CpServices` from the `APP_NAME` check and align with standard SQL script formatting. 
 
- Prevented payload queue records from being created for **WALK-IN** customers. 
  
  - Payload stored procedures were updated so documents associated with the **WALK-IN** customer are no longer inserted into the payload queue. 
 
- Prevented Klaviyo-related errors from interrupting Touchscreen transactions. 
  
  - Instead of displaying pop-up errors in Touchscreen, errors are now written to the message center so transactions can complete successfully and issues can be addressed later. 
 
- Improved email validation logic when creating Klaviyo customer records. 
  
  - Updated the `USER_TR_USER_KLAVIYO_CUST_U` trigger to validate email address format. 
  - Previously, only blank email addresses were checked. 
  - The connector now verifies that the email address in `AR_CUST` is both non-blank and in a valid format before creating a Klaviyo customer record. If invalid, an error is raised and the Klaviyo customer record is not created. 
 
- Resolved deadlock issues during drawer posting. 
   
  - Added a `NOLOCK` hint in `ProfileService.UploadProfileToKlaviyo` when retrieving custom field mappings where the table name is not `AR_CUST`. 
 
- Fixed duplicate customer errors when updating Counterpoint customer records. 
   
  - Updated `AR_CUST` account name triggers to check for the existence of the customer number before processing updates. 
 
- Improved compatibility for rebuilds involving offline stations. 
  
  - Updated payload stored procedures to use `sp_executesql` when running insert scripts that use JSON functions. 
  - This allows rebuilds for offline stations running SQL Server 2012. 
  - Note: The server environment must be running SQL Server 2016 or newer. 
