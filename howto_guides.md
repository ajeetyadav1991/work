# 📚 How-To Guides - DPN Anomaly Detection System

Complete step-by-step guides for all system operations.

## 📋 Table of Contents

### Getting Started
- [How to Access the System](#how-to-access-the-system)
- [How to Navigate the Interface](#how-to-navigate-the-interface)
- [How to Understand Row Colors](#how-to-understand-row-colors)

### Basic Operations
- [How to Filter Anomalies](#how-to-filter-anomalies)
- [How to Search Paths](#how-to-search-paths)
- [How to Sort Data](#how-to-sort-data)
- [How to Reset Filters](#how-to-reset-filters)

### Investigation Phase
- [How to Select an Anomaly](#how-to-select-an-anomaly)
- [How to Classify an Anomaly](#how-to-classify-an-anomaly)
- [How to Enter iREV Values](#how-to-enter-irev-values)
- [How to Submit Investigation](#how-to-submit-investigation)

### Resolution Phase
- [How to Assign a Team](#how-to-assign-a-team)
- [How to Set Resolution Date](#how-to-set-resolution-date)
- [How to Close a Case](#how-to-close-a-case)

### Advanced Features
- [How to View Illustrative Cases](#how-to-view-illustrative-cases)
- [How to Analyze Sessions](#how-to-analyze-sessions)
- [How to Use Analytics Dashboard](#how-to-use-analytics-dashboard)
- [How to Export Data](#how-to-export-data)

### Troubleshooting
- [Common Issues](#common-issues)
- [FAQ](#faq)

---

## Getting Started

### How to Access the System

**Steps:**

1. **Open the file in your browser:**
   ```bash
   # macOS
   open anomaly.html
   
   # Windows
   start anomaly.html
   
   # Linux
   xdg-open anomaly.html
   ```

2. **Verify successful load:**
   - Page title: "DPN Anomaly Detection - Production v12.0"
   - Header shows: "🚀 DPN Anomaly Detection"
   - 5 tabs visible
   - No console errors (F12 → Console)

**Troubleshooting:**
- Blank page? Check browser console (F12)
- Missing filters? Try hard refresh (Ctrl+Shift+R)

---

### How to Navigate the Interface

**5 Main Tabs:**

| Tab | Purpose | When to Use |
|-----|---------|-------------|
| **Anomaly Inventory** | Main workspace | Daily investigation work |
| **Illustrative Cases** | Visitor details | See affected sessions |
| **Session Analysis** | Page-by-page journey | Deep-dive analysis |
| **Investigation Log** | Audit trail | Review history |
| **Outcome Analytics** | Charts & stats | Management reports |

---

### How to Understand Row Colors

**Color Coding:**

| Color | Status | Meaning |
|-------|--------|---------|
| **⚪ White** | Not Investigated | New anomaly |
| **🟢 Green** | Classified | Awaiting resolution |
| **🔵 Blue** | Resolved | Case closed |

**Visual Example:**
```
┌─────────────────────────────────┐
│ a1 │ ... │ N/A           │     │ ← White (new)
│ a2 │ ... │ Tech Error    │ ... │ ← Green (classified)
│ a3 │ ... │ Marketing     │ ... │ ← Blue (closed)
└─────────────────────────────────┘
```

---

## Basic Operations

### How to Filter Anomalies

**Single Filter Example:**

1. Click "Filter Week Dates" dropdown
2. Uncheck "Select All"
3. Check only "2024-01-15"
4. Grid updates automatically

**Multiple Filters Example:**

1. **Team:** Uncheck All → Check "US"
2. **Device:** Uncheck All → Check "mobile"
3. **Investigation Phase:** Check only "Open"
4. Grid shows records matching ALL criteria

**Filter Display:**
- "All" = Everything selected
- "3 selected" = Partial selection
- "None" = Nothing (no results)

---

### How to Search Paths

**For 100+ paths, use the search box:**

1. Click "Search Paths" dropdown
2. Type "checkout" in search box
3. List filters in real-time
4. Uncheck "Select All"
5. Check "/checkout"
6. Click outside to close

```
┌─────────────────────────────────┐
│ 🔍 Type to filter paths...      │ ← Type here
├─────────────────────────────────┤
│ ☐ /checkout              ← Shows
│ ☐ /checkout/final        ← Shows
│   /home                  ← Hidden
└─────────────────────────────────┘
```

---

### How to Sort Data

**Steps:**

1. Click column header (e.g., "Week")
2. First click: **Week ▲** (ascending)
3. Second click: **Week ▼** (descending)
4. Click different column to sort by that

**Sortable Columns:**
- Row ID, Week, Path, Journey Type, Team, Business Unit, Device, Completed, Hard Anomaly

**Non-Sortable:**
- Anomaly Type, iREV Value, Team Responsible, Resolution Date (from log)

---

### How to Reset Filters

**Quick Reset:**

1. Click "🔄 Reset Filters" button (top-right)
2. All filters return to "Select All"
3. Sort returns to "Row ID ascending"
4. Pagination returns to page 1
5. Toast appears: "All grid filters reset"

**Use When:**
- Stuck with "0 records"
- Starting new investigation
- Demonstrating system

---

## Investigation Phase

### How to Select an Anomaly

**Steps:**

1. Locate row in grid (e.g., "a1")
2. Click anywhere on the row
3. **Observe:**
   - Row highlights **blue**
   - Control panel activates
   - "Outcome of Investigation Phase" enables

**To change selection:** Click different row

---

### How to Classify an Anomaly

**Steps:**

1. **Select row** (blue highlight)
2. **Scroll to "Outcome of Investigation Phase"**
3. **Click "Choose Anomaly Type" dropdown**
4. **Select classification** (e.g., "Technical Error")

**10 Classification Types:**
- Technical Error
- Previously Anomalies Fixed
- New Tag/Page
- Limited-time-offers (LTO)
- Marketing Change
- Minor Anomalies
- False Positives
- Past Changes
- Unknown Causes
- Missing Tags

---

### How to Enter iREV Values

**iREV = Incremental Revenue Impact ($)**

**Steps:**

1. Locate "Anomaly iREV value (in $)" field
2. Enter value: `450.5`
3. **Or leave blank** (defaults to 0.0000)

**Validation Rules:**

| Input | Valid? | Result |
|-------|--------|--------|
| `450.5` | ✅ | Stored as 450.5000 |
| `450.500001` | ❌ | Error: Max 5 decimals |
| `-50` | ❌ | Error: Must be positive |
| (blank) | ✅ | Defaults to 0.0000 |

---

### How to Submit Investigation

**Steps:**

1. **Verify:**
   - Row selected
   - Anomaly Type chosen
   - iREV entered (or blank)

2. **Click "Submit Anomaly Type"**

3. **⚠️ WARNING MODAL appears:**
   ```
   ┌────────────────────────────────────┐
   │ Are you sure you want to classify? │
   │                                     │
   │ Path: /home                         │
   │ Anomaly Type: Technical Error       │
   │ iREV Value: 450.5000                │
   │                                     │
   │ ⚠️ THIS CANNOT BE UNDONE ⚠️        │
   │                                     │
   │ [Cancel] [Yes, Submit]             │
   └────────────────────────────────────┘
   ```

4. **Click "Yes, Submit Anomaly Type"**

5. **Result:**
   - ✅ Row turns **GREEN**
   - ✅ "Anomaly Type" column populated
   - ✅ "iREV Value ($)" column shows amount
   - ✅ Record appears in Investigation Log
   - ✅ Resolution Phase section enables

**Important:** Cannot undo or edit after submission!

---

## Resolution Phase

### How to Assign a Team

**Prerequisites:** Row must be classified (green)

**Steps:**

1. **Select classified row**
2. **Scroll to "Outcome of Resolution Phase"**
3. **Click "Team Responsible to Fix" dropdown**
4. **Select team:**
   - tech-team-alpha@company.com
   - engineering-prod-support@company.com
   - marketing-analytics@company.com
   - data-integrity-team@company.com
   - platform-services@company.com

5. **Team saved temporarily** (not submitted yet)

---

### How to Set Resolution Date

**Steps:**

1. **Locate "Date of Resolution" field**
2. **Click the date picker**
3. **Select date from calendar**
4. **Or type manually:** `2024-11-20`

**Validation:**
- ✅ Year 1000-9999: Valid
- ❌ Year < 1000 or > 9999: Error
- ❌ Blank: Error (required)

---

### How to Close a Case

**Prerequisites:**
- Row classified (green)
- Team assigned
- Resolution date set

**Steps:**

1. **Click "Close Case" button**

2. **⚠️ WARNING MODAL appears:**
   ```
   ┌────────────────────────────────────┐
   │ Are you sure you want to close?    │
   │                                     │
   │ Path: /home                         │
   │ Anomaly Type: Technical Error       │
   │ Team Responsible: tech-team-alpha  │
   │ Resolution Date: 2024-11-20         │
   │                                     │
   │ ⚠️ THIS CANNOT BE UNDONE ⚠️        │
   │                                     │
   │ [Cancel] [Yes, Close Case]         │
   └────────────────────────────────────┘
   ```

3. **Click "Yes, Close Case"**

4. **Result:**
   - ✅ Row turns **BLUE**
   - ✅ "Team Responsible" column populated
   - ✅ "Resolution Date" column populated
   - ✅ All controls **disabled** (immutable)
   - ✅ Investigation Log updated

**Complete Workflow:**
```
White → Green → Blue
(New) → (Classified) → (Resolved)
```

---

## Advanced Features

### How to View Illustrative Cases

**Purpose:** See 20 random visitor samples for an anomaly

**Steps:**

1. **Click any row** in Anomaly Inventory
2. **System auto-loads data**
3. **Click "Illustrative Cases" tab**

4. **View:**
   - Total cases count
   - 20 random samples (if total > 20)
   - Visitor details table
   - Purple clickable Visitor IDs

**Table Columns:**
- Row ID, Week, Path
- Journey Type, Team, Business Unit, Device
- Visitor Id, Visitor No, Visitor Page No
- MCV ID, Agent ID, Correlation ID
- URL Referral, ContentSquare URL Link

**Sampling:**
- If ≤20 cases: Shows all
- If >20 cases: Shows 20 random (Fisher-Yates shuffle)

---

### How to Analyze Sessions

**Purpose:** Page-by-page visitor journey reconstruction

**Steps:**

1. **Open Illustrative Cases** (see above)
2. **Click any purple Visitor ID link**
3. **System switches to "Session Analysis" tab**

4. **View:**
   - Visitor ID header
   - Page views count
   - Visits count
   - Session event table

**Table Shows:**
- visitor id
- visitor no
- page no
- post_evar30 (event type)
- post_evar50 (URL)
- post_evar74 (journey state)
- post_prop22 (error indicator)

**Navigation:**
- "← Back to Illustrative Cases"
- "🏠 Home Page"

---

### How to Use Analytics Dashboard

**Purpose:** Charts and summary statistics

**Steps:**

1. **Click "Outcome Analytics" tab**
2. **Wait for charts to render** (2-3 seconds)

**4 Visualizations:**

1. **Pie Chart:** Classification type distribution
2. **Bar Chart:** Classifications by path
3. **Bar Chart:** Total iREV by team (US/India)
4. **Horizontal Bar:** Total iREV by responsible team

**Summary Statistics Cards:**
- Total Classified Cases
- Total Closed Cases
- Unique Paths Affected
- Classifications Used
- Total iREV Value ($)

**Use Cases:**
- Management reports
- Team performance tracking
- Identify trends

---

### How to Export Data

**Export Filtered View to CSV/JSON**

**Steps:**

1. **Apply desired filters** in Anomaly Inventory
2. **Click export button:**
   - "📊 Export CSV" → Downloads CSV file
   - "📄 Export JSON" → Downloads JSON file

3. **File downloads** with timestamp in name:
   - `filtered_view_2024-11-13.csv`
   - `filtered_view_2024-11-13.json`

**Exported Data Includes:**
- All visible columns
- Anomaly Type (from log)
- iREV Value (from log)
- Team Responsible (from log)
- Resolution Date (from log)

**CSV Format:**
```csv
Row ID,Week,Path,Journey Type,Team,Anomaly Type,iREV Value ($)
a1,2024-01-15,/home,Basic,US,Technical Error,450.50
a2,2024-01-15,/home,Upgraded,US,N/A,N/A
```

**JSON Format:**
```json
[
  {
    "Row ID": "a1",
    "Week": "2024-01-15",
    "Path": "/home",
    "Anomaly Type": "Technical Error",
    "iREV Value ($)": "450.50"
  }
]
```

---

## Troubleshooting

### Common Issues

#### Issue 1: "Showing 0-0 of 0 records"

**Cause:** Conflicting filters (no records match all criteria)

**Solution:**
1. Click "🔄 Reset Filters"
2. Apply filters one at a time
3. Check filter combinations make sense

---

#### Issue 2: Cannot Submit Classification

**Cause:** "Submit Anomaly Type" button is disabled

**Checklist:**
- ✅ Row selected (blue highlight)?
- ✅ Anomaly Type chosen?
- ✅ Row not already classified (white, not green)?

**Solution:** Verify all prerequisites met

---

#### Issue 3: Cannot Close Case

**Cause:** "Close Case" button is disabled

**Checklist:**
- ✅ Row classified (green, not white)?
- ✅ Team selected?
- ✅ Resolution date entered?
- ✅ Row not already closed (green, not blue)?

**Solution:** Complete all resolution fields

---

#### Issue 4: Charts Not Rendering

**Cause:** No classification data available

**Solution:**
1. Classify at least one anomaly
2. Switch to "Outcome Analytics" tab
3. Wait 2-3 seconds for rendering

---

#### Issue 5: iREV Validation Error

**Error:** "iREV value must be a positive number"

**Common Causes:**
- Negative number entered: `-50`
- Non-numeric: `abc`
- Too many decimals: `450.5000001`

**Solution:** Enter valid positive number with max 5 decimals

---

#### Issue 6: Resolution Date Error

**Error:** "Year must be between 1000 and 9999"

**Cause:** Invalid year entered

**Solution:** Use date picker or enter valid date: `2024-11-20`

---

### FAQ

**Q: Can I undo a classification?**  
A: No, classifications are permanent. Double-check before submitting.

---

**Q: Can I edit iREV value after submission?**  
A: No, iREV values are immutable. Enter carefully.

---

**Q: Can I re-classify a row?**  
A: No, each row can only be classified once.

---

**Q: What if I leave iREV blank?**  
A: System defaults to 0.0000 (no financial impact).

---

**Q: Can I close a case without classifying first?**  
A: No, investigation phase must complete before resolution phase.

---

**Q: How many anomalies can I classify at once?**  
A: One at a time. Bulk classification is a planned feature.

---

**Q: Can I filter by classification type?**  
A: Not directly. Use Investigation Phase filter to show classified/unclassified.

---

**Q: Where is the data stored?**  
A: In-memory only (browser session). Refreshing page resets to sample data.

---

**Q: Can I import my own data?**  
A: Currently no. Replace SAMPLE_DATA in code to use custom data.

---

**Q: How do I backup my classifications?**  
A: Export to CSV/JSON regularly. Data lost on page refresh.

---

**Q: What browsers are supported?**  
A: Chrome 90+, Firefox 88+, Safari 14+, Edge 90+

---

**Q: Is there a mobile app?**  
A: No, but the interface is mobile-responsive.

---

**Q: Can multiple users work simultaneously?**  
A: No, single-user system. Multi-user support is planned.

---

**Q: How do I report a bug?**  
A: Open GitHub issue: https://github.com/yourusername/dpn-anomaly-detection/issues

---

**Q: Where can I get help?**  
A: 
- GitHub Discussions
- Email: dpn-anomaly-support@yourcompany.com
- Documentation: README.md, CONTRIBUTING.md

---

## Quick Reference Card

**Essential Shortcuts:**

| Action | Steps |
|--------|-------|
| **Reset View** | Click "🔄 Reset Filters" |
| **Classify** | Select row → Choose type → Enter iREV → Submit |
| **Close Case** | Select row → Choose team → Enter date → Close |
| **Export** | Apply filters → Click "📊 Export CSV/JSON" |
| **View Details** | Click row → Click "Illustrative Cases" tab |
| **Session Analysis** | Illustrative Cases → Click Visitor ID |
| **Analytics** | Click "Outcome Analytics" tab |

---

**Color Code:**
- ⚪ White = New
- 🟢 Green = Classified
- 🔵 Blue = Closed

---

**Validation Rules:**
- iREV: Positive, max 5 decimals
- Date: Year 1000-9999
- Classification: Required
- Team: Required (for close)
- Date: Required (for close)

---

## Need More Help?

📖 **Documentation:**
- [README.md](README.md) - System overview
- [CONTRIBUTING.md](CONTRIBUTING.md) - Development guide
- This guide - User manual

💬 **Support:**
- GitHub Issues (bugs)
- GitHub Discussions (questions)
- Email: dpn-anomaly-support@yourcompany.com

---

<div align="center">

**Happy Investigating! 🔍**

[⬆ Back to Top](#-how-to-guides---dpn-anomaly-detection-system)

</div>