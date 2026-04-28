# Weekly Stock Report Automation

**Scheduled workflow** - Weekly low-stock summary delivered Monday 9 AM to ordering team.

## Overview

Automated Zapier workflow that:
1. Runs every Monday at 9:00 AM
2. Fetches ALL products from Shopify
3. Identifies items with quantity < 50 units
4. Generates formatted HTML table
5. Sends email to team@happy4pets.it
6. Logs report to Google Sheets (audit trail)

- **Status:** ✅ Built & Tested (Live)
- **Tech Stack:** Zapier, Shopify API, Google Sheets, Gmail
- **Trigger:** Zapier Schedule (Monday 9 AM)
- **Recipients:** Ordering team
- **Run Time:** <1 minute (typically 20 seconds)

## The Problem (Before)

**Manual inventory review:**
- Time spent: 45 minutes every Monday
- Process: Manually check Shopify, create spreadsheet
- Error rate: ~15% (items missed)
- Visibility: Only occurs after team remembers to check
- Workflow: Reactive (discover issues after problems occur)

**Result:** Frequent stockouts, missed sales, inefficient ordering

## The Solution (After)

**Automated weekly report:**
- Time spent: 0 minutes (fully automated)
- Process: Zapier fetches, filters, formats, sends
- Error rate: 0% (systematic check)
- Visibility: Report arrives Monday morning
- Workflow: Proactive (team can plan ahead)

**Result:** Better inventory visibility, proactive ordering, time savings

## Architecture
MONDAY 9:00 AM (Zapier Schedule) ↓ [Trigger fires] ↓ Fetch ALL products from Shopify (500+ items) ↓ Filter: quantity < 50 (8-15 items) ↓ Transform to HTML table format ↓ ┌─────────────┴──────────────┐ ↓ ↓ [Google Sheets] [Email Send] Append row with To: team@ summary happy4pets.it ↓ ↓ Audit log Formatted table (timestamp, in email body items_count, date)

## How It Works (Step-by-Step)

### Trigger: Scheduled
- Type: Zapier Schedule
- Frequency: Weekly
- Day: Monday
- Time: 9:00 AM UTC
- Action: Wake up and execute workflow

### Step 1: Fetch Products from Shopify
- API Call: GET /products.json
- Returns: All products (500+) with current inventory
- Data includes: product_id, title, quantity, status

### Step 2: Filter (Quantity < 50)
- Condition: IF product.quantity < 50 THEN include in report
- Result: Filters down to 8-15 items (varies weekly)

### Step 3: Transform to Table
- Format: HTML table with columns
- Columns: Product Name, Current Qty, Reorder Point (50), Days Supply, Status

### Step 4: Append to Google Sheets
- Sheet: "Weekly Stock Reports"
- Action: Append new row
- Data logged: Timestamp, Items Count, Report Date, Status

### Step 5: Send Email
- To: team@happy4pets.it
- Subject: Weekly Stock Report - {Date}
- Body: HTML table with all low-stock items
- Delivery: Instant (< 1 minute after trigger)

## Implementation Details

### Zapier Workflow Configuration

**Trigger Setup:**
App: Zapier Schedule
Event: Weekly
Day: Monday
Time: 09:00 AM
Timezone: Europe/Rome

**Step 1 - Shopify:**
App: Shopify
Event: Get Products
Account: happy4pets.myshopify.com
Fields: product_id, title, quantity, status

**Step 2 - Filter:**
Type: Zapier Filter
Condition: quantity < 50
Logic: Continue if true, stop if false

**Step 3 - Google Sheets:**
App: Google Sheets
Event: Append Spreadsheet Row
Spreadsheet: Weekly Stock Reports
Sheet: Sheet1
Columns:
  - Timestamp: {{Zap timestamp}}
  - Items Count: {{Count}}
  - Report Date: {{Date}}
  - Status: SUCCESS

**Step 4 - Gmail:**
App: Gmail
Event: Send Email
To: team@happy4pets.it
Subject: Weekly Stock Report - {{Date}}
Body: {{HTML table}}

## Test Results (4 Weeks Pilot)

Week 1 (Apr 7): 18 sec execution, 10 items found, ✓ SUCCESS
Week 2 (Apr 14): 22 sec execution, 15 items found, ✓ SUCCESS
Week 3 (Apr 21): 19 sec execution, 12 items found, ✓ SUCCESS
Week 4 (Apr 27): 20 sec execution, 12 items found, ✓ SUCCESS

**Summary:**
- Total runs: 4
- Success rate: 100%
- Avg execution time: 19.75 seconds
- Avg items identified: 12.25
- Uptime: 100%

## Cost & ROI

- Zapier cost: Free
- Google Sheets: Free
- Gmail: Free
- Development time: 8 hours
- Ongoing time: 5 minutes/week
- Time saved: 45 minutes/week

**Monthly ROI:** 3 hours saved per month, cost = $0

## Files in This Repository

- README.md - This documentation
- zapier-flow.json - Complete workflow export
- email-template.html - HTML email template
- test-results.csv - 4-week test data

## Status

✅ **Production Ready** - Tested 4 weeks, 100% uptime
Last tested: April 27, 2026
