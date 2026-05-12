# Rapid POS Klaviyo Connector v3.04.00 Release Notes
Release Date: May 13th, 2026


## New Functionality

### Capitalize Customer Fields Configuration Setting
Added a new Klaviyo configuration setting to support automatic capitalization of customer fields during customer synchronization and processing.

When enabled, customer information such as names and address-related fields can be automatically converted to uppercase formatting to maintain consistency between CounterPoint and Klaviyo customer records.

This enhancement helps improve:
- Data consistency across systems
- Customer search and matching reliability
- Standardized customer record formatting

---

## Enhancements

### Mail Group ID Support for Counterpoint Messaging Accounts
Enhanced the Klaviyo configuration by adding support for **Mail Group ID**.

This enhancement allows messaging accounts in Counterpoint to properly recognize and associate messages with the correct group configuration during synchronization and processing.

Additional improvements include:
- Correct association of messages to configured mail groups
- Resolved issues related to messages not being properly marked as read

This update improves overall reliability and message tracking within the Klaviyo integration workflow.

---

## Bug Fixes and Performance Enhancements

### Allow Multiple Customers with the Same Email Address
Removed validation preventing multiple customers from sharing the same email address.

Previously, customer inserts or updates could fail when another customer record already existed with the same email address. The connector now allows multiple customer records to use the same email address when required by the business workflow.

This improvement provides greater flexibility for:
- Shared family email accounts
- Business accounts with multiple associated contacts
- Multi-customer relationships using a common email address

### Prevent Last Customer Sync Date Updates on Failed Downloads
Updated customer profile download logic to prevent the `LST_CUST_SYNC_DT` field from being updated when an error occurs during customer profile synchronization.

Previously, failed downloads could still update the last sync timestamp, causing synchronization inconsistencies and making troubleshooting more difficult.

The connector now only updates the sync date after a successful customer profile download, improving:
- Sync reliability
- Error recovery handling
- Visibility into failed synchronization attempts
