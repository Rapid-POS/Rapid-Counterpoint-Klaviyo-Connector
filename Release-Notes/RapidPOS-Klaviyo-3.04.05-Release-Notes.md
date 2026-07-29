# RapidPOS Klaviyo Connector v3.04.05 Release Notes

**Release Date:** July 28, 2026

Reworked ticket-history sync setting.

## New Features & Improvements

### Automatic recovery from brief database connection issues
If the connection to your Counterpoint database drops momentarily, whether the connector is starting up or already running, it now waits and retries automatically instead of getting stuck or failing outright.

### Ticket history sync setting reworked
On the Klaviyo configuration screen, "Ticket History End Date" has been replaced with "Documents Up Look Back Days." Instead of picking a fixed cutoff date, you now enter how many days back to look, and that window rolls forward automatically as time passes. "Ticket History Start Date" and "Ticket History Queue Batch Size" have also been renamed to "Documents Up Start Date" and "Documents Up Queue Batch Size" for clarity.

* NOTE: We have set all "Documents Up Look Back Days" to look back 7 days by default. 

### Large customer backlogs now sync gradually
When there's a large backlog of Klaviyo profile changes to pull in, for example after the connector has been offline for a while, it now catches up in manageable chunks across multiple sync cycles instead of trying to pull everything down at once.

## Bug Fixes

### Temporarily failed Klaviyo events now retry instead of getting stuck
Order and ticket events that failed because Klaviyo was temporarily busy or rate-limiting requests were previously marked as permanently failed and never retried. They now retry automatically on a later sync cycle.

### Sync cycles no longer overlap
Previously, if a sync cycle ran long, the next scheduled cycle could start before the first one finished. The connector now always waits for the current cycle to finish before starting the next one.

## Maintenance

* Improved backend reliability and security hardening across the connector's database and Klaviyo sync logic.
