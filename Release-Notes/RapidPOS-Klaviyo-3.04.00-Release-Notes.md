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

### Improved Handling of Duplicate Customer Email Addresses

In this release we updated customer validation logic to prevent order processing interruptions when multiple CounterPoint customers share the same email address.

Previously, customer inserts, updates, or order processing would throw an error message when another customer record already existed in Counterpoint with the same email address and was already associated with a Klaviyo customer profile. In order to proceed the email address would have to be removed or updated to a different email address not already used by another contact in Counterpoint.

The validation has now been adjusted to allow the customer and order workflow to continue successfully within CounterPoint, even when duplicate email addresses exist across multiple customer records.

#### Important Behavior Notes

- Multiple CounterPoint customers may now share the same email address without blocking customer updates or order entry.
- Duplicate email customers are still **not uploaded or synced to Klaviyo**.
- Only the original customer associated with the email address will continue syncing with Klaviyo.
- This change prevents unnecessary workflow interruptions while maintaining Klaviyo profile integrity and duplicate email protection.

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
