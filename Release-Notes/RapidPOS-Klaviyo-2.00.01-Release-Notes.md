# Rapid POS Klaviyo Connector – Release Notes 

## Version 2.00.01  
**Release Date:** July 2, 2024  
 
---
 
## New Functionality 
 
- Updated Auto-Enroll behavior to prevent duplicate Klaviyo customer records 
  
  - When Auto-Enroll is enabled, the connector will no longer attempt to automatically create a Klaviyo customer record if the email address already exists on another Klaviyo customer record within the same Klaviyo account. 
  - The AR_CUST Klaviyo trigger was updated to first check whether the email address already exists on another Klaviyo customer record before attempting to create a new one. 
  - This change applies only to clients who have Auto-Enroll configuration enabled. 
 
---
 
## Bug Fixes and Performance Enhancements 
 
- Reduced the number of properties sent to Klaviyo for metric events. 
  
  - Introduced a new view, `USER_VI_PS_DOC_HDR_KLAVIYO`, to eliminate sending unnecessary data to Klaviyo. 
  - This change helps prevent errors caused by exceeding Klaviyo’s limit of 400 properties per metric, which the connector was occasionally reaching. 
 
- Improved payload stored procedures to enhance reliability and performance. 
 
- Updated custom event field mapping logic to replace single quotes with double quotes in result values, preventing payload-related errors during syncs. 
