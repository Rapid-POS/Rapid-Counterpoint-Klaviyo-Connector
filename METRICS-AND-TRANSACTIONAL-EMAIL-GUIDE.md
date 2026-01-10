# Rapid POS Klaviyo Connector  
## Metrics and Transactional Email Guide

**Version:** 3.3  
**Updated:** January 9, 2026  

---

## Table of Contents

- [1. Overview](#1-overview)
- [2. Klaviyo Metrics from Counterpoint](#2-klaviyo-metrics-from-counterpoint)
  - [2.1 Automatically Synced Metrics](#21-automatically-synced-metrics)
  - [2.2 Accessing Metric Data in Klaviyo](#22-accessing-metric-data-in-klaviyo)
  - [2.3 Metric Structure and Dimensions](#23-metric-structure-and-dimensions)
  - [2.4 Dimension Levels and Filtering Rules](#24-dimension-levels-and-filtering-rules)
  - [2.5 Elevating Detail Fields to Top-Level Dimensions](#25-elevating-detail-fields-to-top-level-dimensions)
  - [2.6 Standard Metric Properties](#26-standard-metric-properties)
  - [2.7 Setting Up Trigger Filters](#27-setting-up-trigger-filters)
- [3. Sending Emails Using Klaviyo](#3-sending-emails-using-klaviyo)
  - [3.1 Disclaimer](#31-disclaimer)
  - [3.2 Marketing vs. Transactional Emails](#32-marketing-vs-transactional-emails)
  - [3.3 Transactional Email Requirements](#33-transactional-email-requirements)
  - [3.4 Personalization Variables and Metric Properties](#34-personalization-variables-and-metric-properties)
  - [3.5 Hints for Transactional Email Receipts](#35-hints-for-transactional-email-receipts)
- [4. Reference Links and Additional Notes](#4-reference-links-and-additional-notes)

---

## 1. Overview

This document provides detailed guidance on **Klaviyo metrics**, **dimensions**, and **transactional email usage** when using the Rapid POS Klaviyo Connector.

It is intended to be a **companion document** to the primary *Rapid POS Klaviyo Connector README*. While the main README focuses on connector configuration, sync behavior, and operational controls, this guide explains:

- How transactional data from Counterpoint is represented in Klaviyo  
- How metrics and dimensions work  
- How this data can be used in Klaviyo flows and email templates  

This document does **not** provide instruction on marketing strategy or email design best practices.

---

## 2. Klaviyo Metrics from Counterpoint

Documents including **tickets**, **orders**, and **layaways** (with **holds** and **quotes** planned for future support) are automatically sent from Counterpoint to Klaviyo as **metrics**.

These metrics can be used to:
- Trigger Klaviyo flows  
- Build segments  
- Generate insights  
- Support transactional and marketing emails  

All metrics appear in Klaviyo with the source labeled as **API**. While this data is always sent, none of it is required to be used unless desired.

---

### 2.1 Automatically Synced Metrics

The following metrics are sent to Klaviyo for customers who have Klaviyo profiles:

#### Ticket Events
- Created  
- Voided  

#### Order Events
- Created  
- Edited  
- Reinstated  
- Released*  
- Canceled  

#### Layaway Events
- Created  
- Edited  
- Reinstated  
- Released*  
- Canceled  

\* **Important Note**  
Order and layaway releases appear in Klaviyo as **Counterpoint Ticket Created** events.  
They can be identified by filtering for:
- `DOC_TYP = T`  
- `IS_REL_TKT = Y`  

Future updates will include support for **hold** and **quote** created and deleted events.

---

### 2.2 Accessing Metric Data in Klaviyo

To view metric data for a customer:

1. Open the customer profile in Klaviyo  
2. Locate the desired event  
3. Click the **three dots** next to the event  
4. Select **Activity details**  

Each field displayed in the activity details represents a **dimension** and its associated value.

---

### 2.3 Metric Structure and Dimensions

Each Klaviyo metric consists of **dimensions**, which are fields sourced from Counterpoint.

#### Document Header Details (Top-Level)
- `DOC_ID` – Unique document ID  
- `STR_ID` – Store ID  
- `TKT_NO` – Ticket number  
- `TKT_TYP` – Ticket type  

#### Document Line Details (Detail-Level)
- `ITEM_NO` – Item number  
- `DESCR` – Item description  

#### Document Payment Details (Detail-Level)
- `PAY_COD` – Pay code  

A complete list of available dimensions, descriptions, and valid values is available upon request in the **Klaviyo Connector Metric Properties Reference Guide**. Fields will only appear in Klaviyo if a value exists in Counterpoint.

**Technical Note**
- Document header data is sourced from a custom view (`USER_VI_PS_DOC_HDR_KLAVIYO`) to reduce the number of fields sent and comply with Klaviyo API rate limits.  
- Document line and payment data are sourced from standard Counterpoint tables (`PS_DOC_LIN`, `PS_DOC_PMT`).

---

### 2.4 Dimension Levels and Filtering Rules

Klaviyo applies different rules depending on the level of a dimension:

- **Top-Level Dimensions**
  - Can be used to filter flow triggers  
  - All document header fields are top-level  

- **Detail-Level (Nested) Dimensions**
  - Can be used as values within emails  
  - Cannot be used to filter flow triggers  
  - Includes document line and payment data  

---

### 2.5 Elevating Detail Fields to Top-Level Dimensions

To work around Klaviyo’s filtering limitations, the connector can resend selected detail-level fields as **top-level dimensions**.

- Values are pulled from document lines  
- Multiple values are combined when applicable  
- Up to **eight** fields can be elevated using the **Klaviyo Custom Properties – Documents Up** configuration  

---

### 2.6 Standard Metric Properties

The following properties are included in a standard deployment:

| Property Source | Level | Nested |
|-----------------|-------|--------|
| `VI_PS_DOC_HDR` fields | Top-Level | No |
| `PS_DOC_LIN` fields | Detail-Level | Yes |
| `PS_DOC_PMT` fields | Detail-Level | Yes |
| Item_Numbers | Top-Level | Yes |
| Item_Descriptions | Top-Level | Yes |
| Item_Categories | Top-Level | Yes |
| Item_Subcategories | Top-Level | Yes |
| Item_Primary_Vendor_Names | Top-Level | Yes |

---

### 2.7 Setting Up Trigger Filters

When building Klaviyo flow triggers:
- Only **top-level dimensions** can be used as trigger filters  
- Selecting a dimension (for example, `Item_Subcategories`) populates available values dynamically  
- Selected values can be used to precisely target flow execution  

---

## 3. Sending Emails Using Klaviyo

### 3.1 Disclaimer

Rapid POS specializes in the **Klaviyo connector and data synchronization** between Counterpoint and Klaviyo.

Rapid staff are **not email marketing experts**. For assistance with:
- Email design  
- Marketing strategy  
- Flow optimization  
- Segmentation best practices  

Please consult a **Klaviyo-experienced marketing professional or agency**.

---

### 3.2 Marketing vs. Transactional Emails

- **Transactional emails** include receipts, order confirmations, and delivery notifications  
- **Marketing emails** include welcome messages, promotions, and lifecycle campaigns  

Klaviyo’s definition:  
https://www.klaviyo.com/blog/transactional-email  

---

### 3.3 Transactional Email Requirements

Klaviyo requires transactional emails to:
- Be triggered by a **metric-based flow**  
- Receive **pre-approval**, which may take up to one business day  

Approval guidance:  
https://help.klaviyo.com/hc/en-us/articles/360003165732  

---

### 3.4 Personalization Variables and Metric Properties

Metric properties and personalization variables can be inserted into Klaviyo email templates.

Important notes:
- Variables are **case sensitive**  
- HTML knowledge is helpful when customizing templates  
- Rapid assistance with metric clarification is billed at **time and materials**  

If a required property is not available, consult Rapid for a quote to sync additional data.

Reference:  
https://help.klaviyo.com/hc/en-us/articles/4408802648731  

---

### 3.5 Hints for Transactional Email Receipts

Klaviyo users are responsible for:
- Creating receipt flows  
- Designing email templates  
- Applying for transactional approval  

**Recommended approach**
- Use the **Counterpoint Ticket Created** metric  
- Optionally filter flows based on:
  - Customer email receipt preferences  
  - Ticket-level receipt delivery indicators  

Some properties may benefit from formatting filters (for example, decimal precision).

Filter reference:  
https://developers.klaviyo.com/en/docs/use_filters_to_customize_variables  

---

## 4. Reference Links and Additional Notes

This document intentionally focuses on **data behavior and usage**, not marketing strategy.

For connector setup, configuration, and operational behavior, refer to the main **Rapid POS Klaviyo Connector README**.

For marketing guidance, consult Klaviyo documentation or a marketing professional with Klaviyo expertise.
