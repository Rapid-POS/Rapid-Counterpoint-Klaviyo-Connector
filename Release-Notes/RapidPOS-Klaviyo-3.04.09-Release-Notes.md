# RapidPOS Klaviyo Connector v3.04.09 Release Notes
**Release Date:** September 06, 2026

Fixes a data-integrity issue for merchants syncing more than one Klaviyo account through the connector.

## Bug Fixes

### Fixed customer data crossing between Klaviyo accounts
For merchants syncing more than one Klaviyo account through the connector, a customer enrolled under more than one account could have their profile data attached to the wrong customer if another account happened to share the same email address or Klaviyo profile ID.

* Downloaded profile changes now correctly stay within the account they belong to.
* Duplicate-email detection now correctly stays within the account it belongs to.
* Ticket history syncing now correctly stays within the account it belongs to.

