# Rapid POS Klaviyo Connector - Version 3.3
Updated 1/8/2026

---

## Overview

The Rapid Klaviyo Connector automatically syncs customer and sales data from Counterpoint to Klaviyo to support targeted email and SMS marketing, customer segmentation, and automated flows. The connector syncs customer profiles and transaction information for customers with a populated **Email Address 1** in Counterpoint.

If configured, **Phone 1** or **Mobile Phone 1** can be included to support Klaviyo SMS marketing.

The following fields are **not used** by the Klaviyo connector:
- Email Address 2  
- Mobile Phone 2  

---

## Minimum System Requirements

- Minimum Counterpoint version: **8.5.6.2**  
- Minimum SQL Server version: **2016**  
- Minimum Windows Server version: **2016**  
- Minimum PowerShell version: **5.1**  

If you would like the Klaviyo connector but your system does not meet these minimum requirements, please consult your Care Team Lead (vCIO) for an upgrade quote.

---
## Table of Contents

- [Minimum System Requirements](#minimum-system-requirements)
- [SECTION 1: Klaviyo Customer Records](#section-1-klaviyo-customer-records)
- [SECTION 2: Klaviyo Field Mapping – Customers Up](#section-2-klaviyo-field-mapping--customers-up)
- [SECTION 3: Klaviyo Field Mapping – Customers Down](#section-3-klaviyo-field-mapping--customers-down)
- [SECTION 4: Klaviyo Custom Properties – Documents Up](#section-4-klaviyo-custom-properties--documents-up)
- [SECTION 5: Queued to Be Sent to Klaviyo](#section-5-queued-to-be-sent-to-klaviyo)
- [SECTION 6: Klaviyo Custom Events](#section-6-klaviyo-custom-events)
- [SECTION 7: Klaviyo Connector Sync Process](#section-7-klaviyo-connector-sync-process)
- [SECTION 8: Customer Profile Syncing Workflow](#section-8-customer-profile-syncing-workflow)
- [SECTION 9: Managing Customer Email and Phone Updates](#section-9-managing-customer-email-and-phone-updates)
- [SECTION 10: Duplicate and Merged Customers](#section-10-duplicate-and-merged-customers)
- [SECTION 11: Klaviyo Configuration](#section-11-klaviyo-configuration)
- [SECTION 12: Additional Connector Operations and Controls](#section-12-additional-connector-operations-and-controls)
- [Conclusion](#conclusion)

---

## SECTION 1: Klaviyo Customer Records

The Klaviyo Connector adds a **Klaviyo Customers** button within Counterpoint, providing access to Klaviyo-specific customer fields directly from the Counterpoint customer record.

![Customer Record - Klaviyo Customers Button](./images/counterpoint-customer-record-klaviyo-customers-button.png)

The email address on the Klaviyo customer record is populated from **Email Address 1** on the Counterpoint customer record.

Depending on configuration, the SMS phone number is populated from either **Phone 1** or **Mobile Phone 1** on the Counterpoint customer record, **only when it meets the following criteria**:
- Contains **exactly 10 numeric digits**
- Does **not** include letters 

If the configured phone number field contains more than or fewer than 10 digits, or if it includes letters, the SMS number will **not** be pushed to Klaviyo.

![Klaviyo Customer Record](./images/counterpoint-klaviyo-customer-record.png)

### Accessing Klaviyo Customer Records

All Klaviyo customer records can also be accessed from:

**Connectors > Klaviyo > Klaviyo Customer Records**

This view allows records to be displayed in **table view**, where filters can be applied to review customers based on their current sync status.

![Klaviyo Customers in Table View](./images/counterpoint-klaviyo-customers-table-view.png)

### Klaviyo Sync Status Codes

Each Klaviyo customer record includes a sync status value indicating its current state in the sync process:

- **0** – Fully synced; nothing pending  
- **1** – Recently created or updated; will sync on the next connector run  
- **2** – Profile is currently in the active sync queue  
- **5** – Invalid email address  
- **6** – Invalid SMS number  
- **9** – Sync error; requires remediation before it can be re-synced  

### Add-on-the-Fly Klaviyo Customer Form (Optional)

An optional **Klaviyo Customers Add-on-the-Fly** form can be configured to give cashiers limited access to Klaviyo customer records, if desired.

Please contact Rapid for a quote if you are interested in a customized add-on-the-fly form for your company.

---

## SECTION 2: Klaviyo Field Mapping – Customers Up

**Best viewed in Table View**

The **Klaviyo Field Mapping – Customers Up** screen provides a user interface for managing which customer fields are sent from Counterpoint up to Klaviyo.

This table defines how customer profile data in Counterpoint maps to Klaviyo profile properties. The standard deployment includes a predefined set of fields that are automatically synced. Adjustments to this table should generally be performed by a programmer.

Calculated fields are not included by default. Any request to add calculated fields must be reviewed and quoted separately by Rapid.

**Note:** **Email Address 1** is a required field and must be sent to Klaviyo.

### Standard Customer Profile Fields Sent to Klaviyo

The following customer fields are included in a standard Klaviyo connector deployment:

1. Email Address 1  
2. Mobile Phone 1  
3. First Name  
4. Last Name  
5. Address 1  
6. Address 2  
7. City  
8. State  
9. Zip Code  
10. Country  
11. A/R Balance  
12. Customer Category  
13. First Sale Date  
14. Last Sale Date  
15. Loyalty Point Balance  
16. Send Email Receipts (Yes/No)

---

## SECTION 3: Klaviyo Field Mapping – Customers Down

**Best viewed in Table View**

The **Klaviyo Field Mapping – Customers Down** table provides a user interface for managing which customer fields are sent from Klaviyo down to Counterpoint.

For clients using web-based sign-up forms or other Klaviyo integrations, this functionality allows customer data entered in Klaviyo to sync back into Counterpoint. This may include:
- Updating (overwriting) existing customer fields in Counterpoint
- Inserting new Counterpoint customer records when no matching record exists

### Default Behavior

In a standard deployment, **no fields are imported** from Klaviyo. All fields in the table are set to **No Action** by default.

Any change to this behavior must be requested by the client, and a programmer will configure the table accordingly.

### Action Types

Each field in the **Customers Down** mapping table is assigned an action type that controls how Klaviyo data is applied in Counterpoint:

- **Insert Only**  
  The field is set by Klaviyo in Counterpoint **only when a new Counterpoint customer record is created**.

- **Update Existing**  
  When the field value changes in Klaviyo, the corresponding field in Counterpoint is updated.  
  This action does **not** set the field during new customer creation.

- **Insert and Update**  
  The field is set by Klaviyo when a new Counterpoint customer record is created, and it is also updated in Counterpoint when the value changes in Klaviyo.

- **No Action**  
  The field is not set or updated by Klaviyo in Counterpoint.

**Note:**  
- The two action types that include **Insert** only function when the configuration option **Insert New Customer Records** is enabled.  
- The two action types that include **Update** will function regardless of that configuration setting.

Use caution when selecting which Klaviyo fields are allowed to overwrite Counterpoint data. Consult with your **Business Analyst (BA)** for guidance before enabling field updates.

### Retain Counterpoint Value if Klaviyo is Empty

This setting controls how blank values from Klaviyo are handled during import:

- **Checked**  
  Counterpoint will **not** be updated with blank values from Klaviyo.  
  This prevents scenarios where a customer leaves a field blank on a web-based sign-up form, unintentionally overwriting existing Counterpoint data.

- **Unchecked**  
  Allows existing Counterpoint values to be overwritten with blank values from Klaviyo.  
  This setting is **not recommended**.

---

## SECTION 4: Klaviyo Custom Properties – Documents Up

Documents including **tickets**, **orders**, and **layaways** (with **holds** and **quotes** planned for a future release) are automatically sent from Counterpoint to Klaviyo. These documents appear in Klaviyo as **metrics**.

This data is sent to Klaviyo for availability and flexibility, it does not need to be actively used. Once present, the metrics can be leveraged to build **flows**, **segments**, or **reporting**, if desired.

Most fields used to populate Klaviyo document metrics are **hard-coded** in the connector. However, additional configurable fields are available through the **Klaviyo Custom Properties – Documents Up** table.

Due to Klaviyo API rate limits, **up to eight** custom properties can be selected in this table.

For more information on how documents are represented in Klaviyo, including the relationship between **metrics** and **dimensions**, refer to **[TBD - INSERT SECTION HERE]**.

---

## SECTION 5: Queued to Be Sent to Klaviyo

**Best viewed in Table View**

When sending metric data to Klaviyo, each document — including **tickets**, **orders**, **layaways**, and (in a future release) **quotes** and **holds** — is first placed into a queue in Counterpoint. Documents are then synced from this queue to Klaviyo during connector runs.

### Queue Status Values

Each document in the queue includes a status value indicating its current state in the sync process:

- **0** – Document has already been synced to Klaviyo; nothing pending  
- **1** – Document has been recently created or updated and will be added to the queue on the next connector run  
- **2** – Document is currently in the active sync queue  
- **9** – Document encountered an error and requires remediation before it can be re-synced

---

## SECTION 6: Klaviyo Custom Events

**Best viewed in Table View**

Klaviyo does not allow flows to be scheduled based on a specific date field. The connector's **custom events** functionality can be used as a workaround to support date-driven automations.

The **Klaviyo Custom Events** table allows specific Counterpoint conditions to trigger custom event metrics that are sent to Klaviyo on a calculated date. These events can then be used to trigger Klaviyo flows.

This table is configured only by a programmer. Please contact Rapid for a quote if you are interested in sending custom events to Klaviyo.

### Example: Delivery Arriving Today Reminder

A ticket profile date stored in Counterpoint can be used to identify a scheduled delivery date. A custom event configured in the connector can push that ticket to Klaviyo **on the delivery date**.

Once received in Klaviyo, a flow can be triggered to send a delivery reminder email or SMS message to the customer.

For details about the **standard event metrics** that are automatically synced to Klaviyo, refer to **[INSERT METRICS SECTION - TBD]**.

---

## SECTION 7: Klaviyo Connector Sync Process

The Klaviyo Connector operates as a **Windows Service**, automatically syncing customer profiles and transactional documents between Counterpoint and Klaviyo.

The connector runs continuously in the background and is responsible for keeping both systems aligned while respecting Klaviyo API rate limits and configured sync rules.

### Sync Intervals

The connector processes different types of data on separate schedules:

- **Customer Profiles**  
  New and updated customer profiles are synced every **15 minutes**.

- **Documents in the Queue**  
  Transactional documents are synced every **1 minute**.  
  This interval is configurable and may be adjusted to prevent Klaviyo rate limiting.

If a document being synced contains a **new customer**, the customer profile is created in Klaviyo **immediately as part of the document sync**. The connector does not wait for the next 15-minute customer profile sync cycle.

---

## SECTION 8: Customer Profile Syncing Workflow

The Klaviyo Connector processes customer profile updates in a defined sequence to ensure that the most recent and authoritative data is preserved between Counterpoint and Klaviyo.

### Step 1: Sync Changes from Klaviyo Down to Counterpoint

The connector first retrieves profile changes made in Klaviyo and evaluates whether those changes should be applied to Counterpoint.

- The connector compares the **date and time** of the most recent profile change in Klaviyo to the **date and time** of the most recent update in Counterpoint.
- The system uses the values from the source with the **most recent timestamp**.

Behavior depends on configuration settings:

- If **Insert/Update Customers** is **enabled**, all configured fields are synced down from Klaviyo to Counterpoint.
- If **Insert/Update Customers** is **disabled**, only **subscription status changes** are synced down.

### Step 2: Sync Changes from Counterpoint Up to Klaviyo

After processing inbound changes, the connector identifies customer records in Counterpoint that have been **created or modified** since the previous sync.

These updates are then pushed up to Klaviyo, ensuring that Klaviyo profiles reflect the most current customer information stored in Counterpoint.

---

## SECTION 9: Managing Customer Email and Phone Updates

When a customer is synced to Klaviyo, the connector stores the associated **Klaviyo Profile ID** on the customer record in Counterpoint. This Profile ID is used for all future updates and ensures that customer history and engagement data are preserved in Klaviyo.

### Updating Email Address and Mobile Phone Number

If **Email Address 1** or **Mobile Phone 1** is updated in Counterpoint for a customer who already has a Klaviyo profile:

- The connector updates the corresponding email address or phone number for the **existing Klaviyo Profile ID**.
- The customer retains their full Klaviyo profile history, including events, metrics, and flow activity.

This approach ensures continuity in Klaviyo even when customer contact information is updated over time.

---

## SECTION 10: Duplicate and Merged Customers

The Klaviyo Connector enforces rules to prevent duplicate Klaviyo profiles and to maintain data integrity when customer records are merged in Counterpoint.

### Duplicate Email Addresses in Counterpoint

If two customers in Counterpoint share the same email address, only **one** customer record can be associated with a Klaviyo profile for a given Klaviyo account.

- Counterpoint will not allow the creation of a second Klaviyo profile using the same email address.
- If a user attempts to force the creation of another Klaviyo profile, the connector will return an error and prevent the sync.

### Merging Customers in Counterpoint

When two customer records are merged in Counterpoint:

- The Klaviyo customer record associated with the **“To”** customer (the record being retained) remains linked to the Klaviyo profile.
- If the **“From”** customer had an associated Klaviyo customer record, that record becomes detached from any active customer.

It is recommended to **manually delete** the detached Klaviyo customer record after the merge. Otherwise, it will remain in Counterpoint with no functional association to an active customer profile.  

---

## SECTION 11: Klaviyo Configuration

The Klaviyo Connector includes a user interface for managing configuration options that control how the connector interacts with Klaviyo and Counterpoint.  

For clients who use **multiple Klaviyo accounts**, a separate configuration record will exist for each account.

### Account Name
- Identifies the Klaviyo account.
- Especially important for companies with more than one Klaviyo account (for example, separate accounts for a retail store and a restaurant).

### Is Enabled?
- Used to temporarily disable the connector while troubleshooting or testing.

### Workgroup ID
- Workgroup **231** will be created and used when inserting new customer records from Klaviyo into Counterpoint.
- The associated Customer Template for this workgroup is applied during customer creation.

### Auto Create Klaviyo Profile
Controls when Klaviyo customer records are automatically created from Counterpoint.

- **EMAIL ONLY**  
  A Klaviyo customer record is automatically created when a customer is added to Counterpoint with **Email Address 1**.  
  - If **Mobile Phone 1** is also present, it will be included on the Klaviyo profile.
  - SMS subscription configuration is handled separately.

- **NO**  
  Klaviyo customer records must be created manually.

- **BOTH**  
  A Klaviyo customer record is automatically created only when both **Email Address 1** *and* **Mobile Phone 1** are present.

- **Note:**  
  **SMS ONLY** is currently a placeholder for potential future development. At this time, **all customers must have an email address** to be synced to Klaviyo.

### Always Send Email Receipt Default

The **Send Email Receipt** flag on the Counterpoint customer record does not have standalone functionality within Counterpoint. Instead, it is sent to Klaviyo and can be used to control Klaviyo flows related to receipt delivery.

- **Checked**  
  Automatically flags **Send Email Receipt** on the Klaviyo customer record.  
  This establishes a default behavior indicating that receipts should be emailed for that customer. Individual customer records can still be adjusted manually.

- **Unchecked**  
  The **Send Email Receipt** flag must be set manually on each customer record.

#### Using a Ticket-Level Receipt Indicator (Advanced Option)

Some clients choose to control receipt delivery at the **ticket level** rather than at the customer level by using a custom ticket header field, commonly named:

`USER_RCPT_DELIV_METHD`

This field is typically populated with a value indicating how the receipt should be delivered for that **specific ticket**, such as:
- **E** – Email receipt
- **P** – Print receipt

When this ticket-level indicator is used:
- Receipt delivery is determined **per ticket**, independent of the customer’s **Send Email Receipt** setting.
- The **Always Send Email Receipt Default** configuration can be ignored as it applies only to the customer record default.
- Klaviyo flows can be built to evaluate this ticket-level value to decide whether a receipt email should be sent.

### Counterpoint Email & SMS Lists

These lists are used to manage subscriptions and control which customer profiles receive **email and SMS communications** originating from Counterpoint data.

During initial setup:
- If no existing lists are specified, the connector will automatically create a **Counterpoint Email List**  and a **Counterpoint SMS List** in Klaviyo.

For clients who already use Klaviyo, the connector can be configured to use one or more **existing Klaviyo lists** instead of creating new ones. This is common when companies want Counterpoint data to flow into an established marketing list.

- Klaviyo **List IDs** are configured in this section.
- List IDs can be located in Klaviyo under **List Settings > List Details**.

### Auto Opt-In by Default
Optional automation designed to reduce manual steps during customer creation.

- **Checked**  
  The opt-in box is automatically flagged when creating a new Klaviyo customer record.

- **Unchecked**  
  The opt-in box must be manually selected.

### Opt-In Definition
- Currently, the only supported opt-in definition is **Subscribed**.

Behavior depends on Klaviyo list settings:
- **Single opt-in lists** typically accept the subscription.
- **Double opt-in lists** often set the subscription to *Never Subscribed* until the customer confirms via email or SMS.
- Klaviyo may reject subscription requests for suppressed profiles, previously unsubscribed contacts, or invalid email addresses.  
  Rapid can send the request, but cannot control Klaviyo’s response.

### Opt-Out Definition
Defines how opt-outs are sent to Klaviyo:

- **Unsubscribed**  
  Opt-outs are pushed to Klaviyo as unsubscribed from the list.

- **Suppressed**  
  Opt-outs are pushed as suppressed profiles.  
  This typically affects email lists only and does not appear to impact SMS lists.

### SMS Country Code
- Klaviyo requires all SMS phone numbers to include a country code.
- Examples:
  - **+1** (United States and Canada)
  - **+52** (Mexico)

### Insert New Customer Records
Controls whether Klaviyo profiles can create new Counterpoint customer records.

- **Checked**  
  Klaviyo profiles that do not match an existing Counterpoint customer will be inserted into Counterpoint.
  - Customer records are created using fields configured in **Klaviyo Field Mapping – Customers Down** with an **Insert** action.
  - Commonly used when customer data originates from website sign-up forms.
  - Matching logic:
    1. Klaviyo Profile ID
    2. Email Address 1
  - If no match is found in either Counterpoint (`AR_CUST`) or Klaviyo customer records (`USER_KLAVIYO_CUST`), a new Counterpoint customer record is created.
  - All profiles changed since the last sync are evaluated, regardless of Klaviyo list membership.

- **Unchecked**  
  New Counterpoint customer records will not be created by the connector.

**Important Notes:**
- Updating existing Counterpoint customer records is **independent** of this setting. Refer to **SECTION XXX: Klaviyo Field Mapping – Customers Down**.
- Importing customers can result in **duplicate records** if email addresses were not previously captured in Counterpoint.
- Duplicates can be merged manually, but this can be especially problematic for clients using **DL Scan** or **3310 forms**.
- Consult with your **Business Analyst**, **vCIO**, or **Project Manager** before enabling this feature.

### Ticket History Start and End Dates
- Enables syncing of previously posted (historical) tickets to Klaviyo.
- Intended for **one-time use only**.

**Important:**
- Do **not** enable this yourself. Consult with your Business Analyst.
- Running history syncs multiple times with overlapping dates will result in **duplicate data** in Klaviyo.
- If either date is left blank, historical tickets will not sync.
- After the specified date range is processed, the configuration automatically resets to null.

### Other Configuration Options
Additional configuration fields exist for internal use by Rapid programmers. These options are used to optimize performance or assist with troubleshooting and should not be modified by end users.

---

## SECTION 12: Additional Connector Operations and Controls

This section covers additional utilities and operational controls available for the Klaviyo Connector. These tools are primarily intended for monitoring, troubleshooting, and administrative use.

### Mark All Klaviyo Messages as Read

The **Mark All Klaviyo Messages as Read** option allows users to retain connector messages while suppressing repeated pop-up alerts in Counterpoint.

This is especially useful in scenarios such as:
- Repeated error messages following a temporary internet outage
- High-volume alert conditions that have already been reviewed

Messages remain accessible for later review even after they are marked as read.

### Klaviyo Customer Status View

**Best viewed in Table View**

The **Klaviyo Customer Status View** provides a summary view showing:
- How many customer records are successfully syncing to Klaviyo
- How many customers have encountered errors that require remediation

This utility is helpful for quickly identifying sync health and prioritizing follow-up actions.

For details on the meaning of each customer sync status value, refer back to **SECTION 1: Klaviyo Customer Records**.

---

## Conclusion

The Rapid Klaviyo Connector streamlines the exchange of customer profiles and transactional data between Counterpoint and Klaviyo, enabling powerful email and SMS marketing, accurate segmentation, and automated flows.

Before go-live, review configuration settings, field mappings, and list configurations to ensure customer data and subscription preferences are handled correctly. After deployment, monitor customer and document sync status views to identify invalid data or records requiring remediation.

For assistance with configuration changes, custom field mapping, event setup, or troubleshooting, contact Rapid Support.  

  
