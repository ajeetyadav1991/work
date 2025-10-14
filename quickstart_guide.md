# 🚀 Quick Start Guide

## Get Started in 5 Minutes

This guide will help you start using the DPN Anomaly Detection System immediately.

---

## 📥 Installation

### Option 1: Download & Open (Easiest)

```bash
# 1. Download the HTML file
wget https://raw.githubusercontent.com/yourorg/dpn-anomaly-detection/main/anomaly.html

# 2. Open in browser
open anomaly.html  # Mac
start anomaly.html # Windows
xdg-open anomaly.html # Linux
```

### Option 2: Local Server (Recommended)

```bash
# 1. Clone repository
git clone https://github.com/yourorg/dpn-anomaly-detection.git
cd dpn-anomaly-detection

# 2. Start local server
python3 -m http.server 8000

# 3. Open in browser
# Navigate to: http://localhost:8000/anomaly.html
```

---

## 🎯 Your First Investigation (3 Minutes)

### Step 1: Select Time Period (30 seconds)

1. Look at the **left panel** → "Filter Week Dates"
2. Click the dropdown → **All weeks are pre-selected** ✅
3. Optional: Uncheck weeks you don't need

**Result:** You should see paths appear below with counts like `/home (6)`

---

### Step 2: Select Paths to Investigate (30 seconds)

1. In the left panel → "Search Paths" box
2. Type: `checkout` (case doesn't matter)
3. You'll see only checkout-related paths
4. **Check the boxes** next to paths you want to analyze

**Result:** The right panel now shows data for selected paths

---

### Step 3: Apply Filters (30 seconds)

1. In the **right panel** → Filter dropdowns (Journey Type, Team, Device)
2. Click any dropdown → Select specific values
3. Example: Click "Team" → Uncheck "India" → Only shows US data

**Result:** Table updates instantly with filtered data

---

### Step 4: Investigate a Record (30 seconds)

1. **Click any row** in the table
2. Automatically switches to **"Illustrative Cases"** tab
3. See detailed visitor sessions with:
   - Visitor IDs (click to see page-level details)
   - Page numbers
   - Referral URLs
   - ContentSquare session links

**Result:** You now see granular details for that anomaly

---

### Step 5: Annotate Anomaly (30 seconds)

1. Go back to **"Anomaly Inventory"** tab
2. Click a row to select it
3. Scroll down to **"Classify Anomaly"** section
4. Choose classification (e.g., "Technical Error")
5. Add comment: "iREV impact: $50000"
6. Click **"Submit Anomaly Type"**
7. Confirm in the warning dialog

**Result:** Row turns green → Annotated! ✅

---

### Step 6: Export Data (30 seconds)

1. Apply any filters you want
2. Click **📊 Export CSV** (top right)
3. File downloads immediately
4. Open in Excel/Google Sheets

**Result:** Filtered data exported for further analysis

---

## 🎓 Common Workflows

### Workflow 1: Weekly Anomaly Review

```
Goal: Review and classify all anomalies from last week

1. Select only last week's date
2. Select all paths (or focus on critical paths)
3. Sort by "Hard Anomaly" (click column header)
4. Review high-severity anomalies first
5. Click each row → View details → Classify
6. Export annotated data for reporting
```

**Time:** 15-30 minutes depending on volume

---

### Workflow 2: Path-Specific Deep Dive

```
Goal: Investigate checkout flow issues

1. Search: "checkout"
2. Select all checkout paths
3. Filter Device = "mobile" (if mobile-specific issue)
4. Click rows to see visitor patterns
5. Click Visitor IDs to see full session journeys
6. Identify common patterns
7. Annotate with root cause
```

**Time:** 10-20 minutes

---

### Workflow 3: Batch Annotation

```
Goal: Classify multiple similar anomalies at once

1. Filter to specific criteria (e.g., Team=US, Device=mobile)
2. Click "Batch Mode" button (top left)
3. Check boxes next to similar anomalies
4. Select classification from dropdown
5. Add comment
6. Click "Apply to Selected"
7. Confirm batch operation
```

**Time:** 2-5 minutes for 10+ records

---

### Workflow 4: Trend Analysis

```
Goal: Understand classification distribution

1. Annotate 20+ anomalies first
2. Go to "Outcome Analytics" tab
3. View pie chart: Which categories are most common?
4. View bar chart: Which paths have most anomalies?
5. Use insights to prioritize fixes
```

**Time:** 5 minutes

---

## 🔍 Pro Tips

### Tip 1: Path Search Mastery

```
✅ DO:
- Type partial matches: "check" finds "/checkout"
- Use lowercase: searches are case-insensitive
- Search one word: "home" better than "home page"

❌ DON'T:
- Use wildcards: * not needed
- Quote exactly: "checkout" same as checkout
- Search by count: (6) won't work
```

---

### Tip 2: Efficient Filtering

```
Strategy: Progressive Filtering

1. Start broad: Select all weeks, all paths
2. Apply one filter at a time
3. Observe impact on record count
4. If too many results, add another filter
5. If zero results, remove last filter

Example:
- Start: 500 records
- Filter Team=US: 300 records
- Filter Device=mobile: 150 records ← Good!
- Filter Completed=no: 50 records ← Focused set
```

---

### Tip 3: Reset Smartly

```
The "Reset Filters" button:

✅ Resets:
- Journey Type, Team, Device filters
- Sorting
- Pagination

✅ Preserves:
- Week selections
- Path selections
- Path search query

Use Case: Trying different filter combinations while 
keeping the same time period and paths selected.
```

---

### Tip 4: Annotation Best Practices

```
Good Comments:
✅ "iREV impact: $45000"
✅ "Affects 500 users/week"
✅ "Fixed in release v2.3.1"
✅ "Related to campaign XYZ launch"

Bad Comments:
❌ "error" (too vague)
❌ "idk" (not helpful)
❌ "" (empty - better than nothing, but add context!)
```

---

### Tip 5: Performance Optimization

```
For best performance:

✅ Filter data BEFORE viewing details
  - Select specific weeks (not all)
  - Select specific paths (not all)
  - Reduces data processing

✅ Clear browser cache if sluggish
  - Ctrl+Shift+Del (Windows)
  - Cmd+Shift+Del (Mac)

✅ Refresh page every hour
  - Clears memory
  - Resets caches
  - Improves responsiveness

❌ Don't select 100+ paths at once
  - Slows down rendering
  - Better to filter by week first
```

---

## ⚡ Keyboard Shortcuts

Currently no keyboard shortcuts implemented, but here's how to navigate efficiently:

| Action | Mouse Method | Time Saver |
|--------|-------------|------------|
| **Select path** | Click checkbox | Use search first to reduce list |
| **Sort column** | Click header | Click twice for descending |
| **Next page** | Click "Next" | Or click page number directly |
| **Close dropdown** | Click outside | Or press Escape |
| **Select all (batch)** | Click button | Faster than individual clicks |

---

## 🐛 Quick Troubleshooting

### Issue: No data showing

```
Checklist:
□ Is at least one week selected?
□ Is at least one path selected?
□ Are all filters set to "All"?
□ Try clicking "Reset Filters" button

If still empty: No data exists for your selections
```

---

### Issue: Filters not responding

```
Solution:
1. Wait 1-2 seconds (debouncing active)
2. Check browser console (F12) for errors
3. Refresh page (Ctrl+R)
4. Clear cache and retry
```

---

### Issue: Export not working

```
Solution:
1. Disable popup blocker for this site
2. Reduce filtered data (< 50K rows)
3. Try JSON export instead of CSV
4. Check browser downloads folder
```

---

### Issue: Annotations disappear

```
Expected Behavior:
⚠️ Annotations are stored IN MEMORY only
⚠️ Page refresh will clear all annotations
⚠️ This is temporary until backend is implemented

Workaround:
1. Export Investigation Log before closing
2. Take screenshots of important annotations
3. Use external notes for critical findings
```

---

## 📚 Next Steps

### For Basic Users
- ✅ Complete this quick start
- 📖 Read [User Guide](./docs/USER_GUIDE.md) for advanced features
- 🎥 Watch [Video Tutorial](./docs/videos/) (coming soon)

### For Power Users
- 📖 Read [Advanced Techniques](./docs/ADVANCED.md)
- 🎯 Learn [Batch Operations](./docs/BATCH_OPERATIONS.md)
- 📊 Master [Analytics Dashboard](./docs/ANALYTICS.md)

### For Developers
- 📖 Read main [README.md](./README.md)
- 🏗️ Review [System Architecture](./docs/ARCHITECTURE.md)
- 💻 Check [API Documentation](./docs/API.md) (future)

---

## 💬 Getting Help

**Questions?**
- Check [FAQ](./docs/FAQ.md)
- Post in [Discussions](https://github.com/yourorg/dpn-anomaly-detection/discussions)
- Email: anomaly-support@yourcompany.com

**Found a Bug?**
- Report in [Issues](https://github.com/yourorg/dpn-anomaly-detection/issues)
- Include: Browser, OS, steps to reproduce

**Want to Contribute?**
- Read [Contributing Guide](./CONTRIBUTING.md)
- Start with "good first issue" labels

---

## ✅ You're Ready!

You now know how to:
- ✅ Filter data by weeks and paths
- ✅ Apply multi-column filters
- ✅ Investigate specific anomalies
- ✅ Annotate with classifications
- ✅ Export filtered datasets
- ✅ Use batch operations
- ✅ Troubleshoot common issues

**Time to analyze some anomalies! 🚀**

---

*Last updated: January 2024*
*Version: 7.0*