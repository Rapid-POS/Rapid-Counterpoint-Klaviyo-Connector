# Rapid POS Klaviyo Connector v3.03.00 Release Notes 
 
_Release Date: January 13, 2026_  
 
---
 
## New Functionality

### Updated Functionality for Run Klaviyo Connector (Manually) via Menu Button

- Updated the **Run Klaviyo Connector** menu button to use an internal, stored procedure–driven execution model instead of invoking an external executable.
  - This allows users who have access to the menu button to manually run the connector from any station. Previously, manually running the connector was only reliable when initiated directly from the server.

- Introduced a **Manual Run Connector** action flag within the Klaviyo configuration.
  - The flag is stored in the configuration table and is set when the **Run Klaviyo Connector** menu button is clicked.
  - This flag functions as a one-time action control and remains enabled until it is processed by the connector.

- Added background processing logic to execute the connector based on the action flag.
  - A scheduled process, driven by a CRON-based schedule stored in the configuration table, checks for the **Manual Run Connector** flag at regular intervals.
  - The **Manual Run Connector Execution Time** CRON schedule is configurable from the Klaviyo Configuration screen.
  - If the Klaviyo connector is **not currently running**, it will execute for all configured accounts based on the CRON schedule (typically within one minute), and the action flag will be automatically cleared when execution begins.
  - If the connector **is already running**, the system waits for the current execution to complete, then restarts the connector (typically within one minute) for all configured accounts and clears the action flag.
  - This ensures connector executions do not overlap while still allowing users to safely trigger a manual run.

### Configurable Phone Number Mapping for Klaviyo SMS

- Added a new configuration option to control which phone number is sent to Klaviyo as the SMS number.
  - Introduced a **Phone # for Klaviyo** setting with a drop-down selection of **Mobile Phone 1** or **Phone 1**.
  - For clients using Klaviyo SMS messaging, this determines which Counterpoint phone field is mapped to the Klaviyo customer profile field **SMS Number – Mobile Phone**.
  - The selected phone value is synced to Klaviyo as the SMS number.

### Klaviyo Profile Custom Properties – New Tool for Syncing Calculated Values

- Added a new tool for defining **Klaviyo Profile Custom Properties** that can be calculated and synced to Klaviyo.
  - Values are calculated from ticket history data using:
    - `PS_TKT_HIST` (ticket header data)
    - `PS_TKT_HIST_LIN` (ticket line data)
  - Example use cases include calculating total sales by customer over a specific date range and syncing that value to Klaviyo as a profile custom property.

#### Guided Calculation Definition and Constraints

- The tool includes a guided workflow with the following constraints:
  - **Property Name**
    - Must be unique (primary key)
    - Must not contain spaces
    - Becomes the permanent label for the profile property in Klaviyo and cannot be changed
  - **Data Sources**
    - Limited to `PS_TKT_HIST` and `PS_TKT_HIST_LIN`
  - **Fields**
    - Restricted to numeric fields only
  - **Aggregate Functions**
    - `SUM`, `MAX`, `MIN`, `AVG`, or `COUNT`
  - **Date Range**
    - A begin and end date are required

#### Example Use Case

A client wants to sync the sum of ticket subtotals for all historical tickets between January 1, 2025 and March 31, 2025 (first quarter) for customers with existing Klaviyo profiles.

- **Property Name**: `TicketSubtotalQ1of2025`
- **Table Name**: `PS_TKT_HIST`
- **Column Name**: `SUB_TOT`
- **Aggregate Function**: `SUM`
- **Begin Date**: `01/01/2025`
- **End Date**: `03/31/2025`

#### Automatic View Generation and Sync Behavior

- When a calculated property definition is saved to **Klaviyo Profile Custom Properties** (`USER_KLAVIYO_PROFILE_CUSTOM_PROP`), a database trigger automatically generates a corresponding SQL view.
  - View naming convention:  
    `USER_VI_KLAVIYO_PROFILE_CUSTOM_PROP_[ProfileCustomPropertyName]`
  - Each view returns:
    - Customer Number
    - Calculated output value for the custom property

- When enabled:
  - The view becomes available within **Klaviyo Field Mapping – Customers Up**.
  - Customers in the result set have their Klaviyo Stat set to `1`, triggering a resync to Klaviyo with the additional property.
  - Note: If a customer does not have a calculated value, the property is not synced and will not appear on the Klaviyo profile.

---

## Bug Fixes and Performance Enhancements

### Improved Klaviyo API Connectivity Error Messaging

- Improved the error message displayed when the Klaviyo API cannot be reached, providing clearer guidance on potential causes (such as Klaviyo service outages or local internet connectivity issues) and next steps for contacting Rapid POS Support.

### Improved Handling of Duplicate Customer Email Addresses

- Enhanced logic for handling duplicate customer email addresses during customer updates and Klaviyo syncs.

  - **Scenario 1 – Email already exists in Counterpoint**  
    If an existing customer’s email address is changed to one that already belongs to another customer in Counterpoint (and therefore already has an associated Klaviyo profile), Counterpoint continues to block the update and displays an error. This behavior is expected and unchanged.

  - **Scenario 2 – Email exists in Klaviyo but not in Counterpoint**  
    If an existing customer’s email address is changed to one that already exists in Klaviyo but does not yet exist in Counterpoint, the update is allowed. The Counterpoint customer is automatically reassigned to the existing Klaviyo profile ID, and the sync completes without error.

### Revised Customer Down Sync Order for Email and SMS Phone

- Updated the order of operations for **Customer Down** updates.
  - Email and SMS Phone values are now applied to the Counterpoint customer record first.
  - After the Counterpoint update succeeds, the corresponding Klaviyo customer record is updated to reflect the new values.

### Improved Handling of Special Characters in Item Descriptions

- Improved handling of special characters in item descriptions synced to Klaviyo.
  - Refactored SQL logic in the `USER_KLAVIYO_CUSTOM_PROPERTIES_DOCUMENTS_UP` table to better handle escape characters.
  - Updated update logic so the `CALCULATED_COLUMN_QUERY` is modified only when changes are detected.
  - Properly accounts for:
    - Backslash (`\`)
    - Ampersand (`&`)
    - Angle brackets (`<` and `>`)

### Updated Install Script for Custom Properties Documents Table

- Adjusted the install script for the `USER_KLAVIYO_CUSTOM_PROPERTIES_DOCUMENTS_UP` table.
  - Removed the previous approach of inserting predefined values directly.
  - Introduced a temporary staging table to apply escape character handling prior to insert or update.
  - Updated insertion logic to prevent duplicate records.

### Process ID (PID) Retention During Initial Customer Uploads

- Fixed an issue where the process ID (PID) was not retained during initial Klaviyo customer uploads.
  - Each upload batch is assigned a unique PID used for grouping, rollback, and resync logic.
  - During initial installs, the PID was not being correctly stored, resulting in null PIDs and failed syncs.
  - The connector now correctly retains and persists the PID during initial installs, eliminating the need for manual intervention.

### Corrected Last Sync Date for Initial Customer Creation

- Fixed an issue where the last sync date was not properly set during initial customer creation in Klaviyo.
  - The `USER_KLAVIYO_CUST.USER_KLAVIYO_LST_SYNC_DT` field is now explicitly set at the start of the initial sync.
  - This improves reliability for incremental syncs and provides clearer visibility into customer sync status.

### Refined Retain Counterpoint Value Logic for Calculated Fields

- Refined logic for calculated customer fields when **Retain Counterpoint Value if Klaviyo is Empty** is enabled.
  - Previously, Klaviyo values containing only whitespace could overwrite existing Counterpoint values.
  - Values consisting solely of spaces are now treated as empty.
  - When the retain flag is enabled, whitespace-only values will no longer overwrite meaningful Counterpoint data.
  - This change applies to **Customer Down** Klaviyo field mappings.

---
