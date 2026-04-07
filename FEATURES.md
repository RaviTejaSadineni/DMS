# 🚀 DMS Booking Portal — Feature Enhancement Roadmap

## Total: 152 Features (New + Enhanced)

---

## 🔥 CATEGORY 1: AI-POWERED PDF IMPORT (14 Features)

### Current State
The PDF parser uses regex pattern matching — works for structured PDFs but fails when data is in tables, split across lines, or uses unexpected abbreviations.

### Enhancements

| # | Feature | Description |
|---|---------|-------------|
| 1 | **Azure OpenAI PDF Extraction** | Replace regex parser with Azure OpenAI GPT-4o vision — upload PDF as image, AI reads and extracts all fields regardless of layout (tables, sideways text, scattered data). Configure via `AZURE_OPENAI_ENDPOINT` and `AZURE_OPENAI_KEY` env vars. |
| 2 | **Multi-page PDF intelligence** | AI determines which pages contain booking data vs T&C pages and only processes relevant pages. |
| 3 | **Table-format extraction** | Handle PDFs where data is in HTML-like tables (Hapag-Lloyd, Wan Hai) — AI reads table cells and maps them to form fields. |
| 4 | **Abbreviation resolver** | AI resolves abbreviations: "NSA" → "NHAVA SHEVA", "INMUN" → "MUNDRA", "SGSIN" → "SINGAPORE", "22GP" → "20'GP", "TC20" → "20'TANK". Build a growing abbreviation dictionary in the database. |
| 5 | **Confidence score per field** | Each extracted field gets a confidence score (0-100%). Fields below 70% are highlighted yellow in the form for manual review. |
| 6 | **Merged/multi-booking PDF splitter** | Detect when a single PDF contains multiple booking confirmations (like your merged PDF) and let user pick which booking to import. |
| 7 | **PDF template learning** | After user corrects AI extraction errors, store corrections as training data per carrier. Over time, extraction accuracy improves per carrier format. |
| 8 | **Carrier auto-detection** | Detect carrier from logo/header using AI vision before even parsing text — use carrier-specific extraction rules. |
| 9 | **Handwritten text OCR** | Handle scanned/handwritten delivery orders where text isn't digitally embedded — use Azure Computer Vision OCR. |
| 10 | **PDF preview with field highlighting** | Show the uploaded PDF in a side panel with colored boxes highlighting where each extracted value came from. |
| 11 | **Batch PDF import** | Upload multiple PDFs at once — system creates one booking per PDF, shows summary of all extracted data for review before saving. |
| 12 | **PDF attachment storage** | Store the original PDF in the database (or Azure Blob Storage) linked to the booking — viewable anytime from entries grid. |
| 13 | **Email-to-booking** | Monitor a mailbox (e.g., bookings@dessertmarine.com) — auto-extract PDF attachments and create draft bookings. |
| 14 | **PDF diff comparison** | When a revised booking PDF is uploaded, highlight what changed vs the original (vessel change, ETD change, etc.). |

---

## 📊 CATEGORY 2: DASHBOARD & KPI ANALYTICS (18 Features)

| # | Feature | Description |
|---|---------|-------------|
| 15 | **Monthly/Weekly/Daily toggle** | Switch dashboard view between daily, weekly, monthly aggregation. |
| 16 | **KPI comparison — period over period** | Compare this week vs last week, this month vs last month — show % change arrows (↑12%, ↓5%). |
| 17 | **KPI comparison — location vs location** | Side-by-side comparison: MUMBAI vs GUJARAT metrics, volumes, revenue. |
| 18 | **Line-wise performance chart** | Bar/pie chart showing bookings per shipping line (CMA CGM: 45, Maersk: 32, etc.). |
| 19 | **Customer-wise volume trend** | Line chart showing top 10 customers' booking volumes over time. |
| 20 | **Vessel utilization tracker** | Track how many bookings per vessel — identify underutilized vs overbooked vessels. |
| 21 | **Port pair heatmap** | Visual heatmap showing most common POL→POD trade routes. |
| 22 | **SLA/TAT tracking** | Track turnaround time: booking creation → SI filed → first print → BL released. Show average days per stage. |
| 23 | **Pending aging analysis** | Show how long entries have been stuck at each stage (SI pending >3 days highlighted red). |
| 24 | **DG cargo ratio** | Percentage of HAZ vs non-HAZ bookings, trend over time. |
| 25 | **Equipment type distribution** | Pie chart: 20'DV vs 40'HC vs 20'TK vs others. |
| 26 | **Revenue estimator** | If freight rates are stored in Locals, estimate total revenue per period. |
| 27 | **Booking funnel visualization** | Funnel chart: Total Bookings → SI Filed → First Print → Corrections → BL Released → Completed. |
| 28 | **Real-time dashboard auto-refresh** | Dashboard auto-refreshes every 60 seconds without manual reload. |
| 29 | **Dashboard date presets** | Quick buttons: Today, Yesterday, This Week, Last Week, This Month, Last 30 Days, Custom. |
| 30 | **Export dashboard as PDF report** | One-click export entire dashboard (charts + metrics) as a formatted PDF report. |
| 31 | **Dashboard widgets — drag & reorder** | Let users drag dashboard cards and charts to customize their layout. Save preference. |
| 32 | **Nomination vs regular split** | Show nominated vs regular bookings ratio on dashboard. |

---

## 📋 CATEGORY 3: ADD BOOKING ENHANCEMENTS (15 Features)

| # | Feature | Description |
|---|---------|-------------|
| 33 | **Auto-fill customer details** | When customer is selected from dropdown, auto-fill contact person, email, sales person from customer master. |
| 34 | **Booking template/preset** | Save frequently used booking combinations as templates (e.g., "Standard CMA CGM NHAVA→DALIAN 40HC") — one-click fill. |
| 35 | **Duplicate booking detection** | Beyond exact match — fuzzy match: same customer + same POL + same POD + same ETD = warning "Similar booking exists". |
| 36 | **Booking number format validation** | Per-carrier booking number format validation (CMA: starts with AMC, Maersk: 9 digits, etc.). |
| 37 | **ETD business day validation** | Warn if ETD falls on a weekend or known port holiday. |
| 38 | **Auto-calculate booking validity** | Based on carrier rules, auto-set validity (Arkas: 5 days from issue, CMA: from/to date range, etc.). |
| 39 | **Port cut-off auto-suggest** | Based on historical data for same vessel/port, suggest likely cut-off dates. |
| 40 | **Multi-container booking** | Support multiple container types in one booking (3x20'DV + 2x40'HC) — already partially done, enhance UI. |
| 41 | **Container number validation** | Validate container number format (4 letters + 7 digits + check digit) per ISO 6346. |
| 42 | **Cargo weight limit warning** | Based on equipment type, warn if declared weight exceeds payload capacity (20'DV max ~21,700 kg). |
| 43 | **Quick-add recent bookings** | "Book Again" button on completed entries — pre-fills form with same customer/route/line. |
| 44 | **Booking draft/save later** | Save incomplete bookings as "Draft" status — resume filling later. |
| 45 | **Required fields per carrier** | Different carriers require different fields — dynamically adjust required fields based on selected line. |
| 46 | **FPOD country auto-detect** | When FPOD is selected/typed, auto-detect and fill the country from the master data or a port database. |
| 47 | **Inline master data add** | Currently exists — enhance with immediate validation feedback and duplicate prevention toast. |

---

## 📑 CATEGORY 4: VIEW ENTRIES / DATA GRID (16 Features)

| # | Feature | Description |
|---|---------|-------------|
| 48 | **Column visibility toggle** | Let users show/hide columns in the DataGrid — save preference to localStorage. |
| 49 | **Column reorder** | Drag columns to reorder — save layout preference. |
| 50 | **Row color coding by status** | Color rows: Yellow = pending SI, Blue = SI filed, Orange = first print, Green = BL released. |
| 51 | **Bulk status update** | Select multiple rows → bulk update a field (e.g., mark 10 entries as "SI Filed" at once). |
| 52 | **Bulk delete** | Select multiple rows → delete all selected with confirmation. |
| 53 | **Inline date picker** | For date fields in the grid, show a date picker popup instead of text input. |
| 54 | **Advanced filter builder** | Multi-condition filter: "Line = CMA CGM AND ETD > 2026-04-01 AND SI Filed = No". |
| 55 | **Saved filter presets** | Save commonly used filters as named presets ("My Pending CMA Bookings"). |
| 56 | **Sort by multiple columns** | Click column headers to sort — hold Shift to add secondary/tertiary sort. |
| 57 | **Frozen columns** | Pin first 2-3 columns (Location, Customer, Booking No) while scrolling horizontally. |
| 58 | **Row expand/detail panel** | Click a row to expand and see all details (equipment breakdown, audit trail, remarks). |
| 59 | **Cell change history** | Click a cell to see its change history (who changed it, from what to what, when). |
| 60 | **Conditional formatting** | ETD in the past → red. Booking validity expired → strikethrough. Container No empty → yellow. |
| 61 | **Copy row to clipboard** | Right-click → "Copy as text" — copies row data in a formatted way for pasting into emails. |
| 62 | **Print-friendly view** | "Print" button generates a clean, printable table view. |
| 63 | **Pagination with jump-to-page** | For large datasets, show page numbers and allow jumping to specific pages. |

---

## ✅ CATEGORY 5: COMPLETED FILES (6 Features)

| # | Feature | Description |
|---|---------|-------------|
| 64 | **Revert to active** | Undo BL Released — move entry back from completed to active (with confirmation). |
| 65 | **Post-completion fields** | Add editable fields for completed entries: ETA Destination, Courier Details, Invoice No, OBL Tracking. |
| 66 | **Completed files archive** | Auto-archive entries older than 90 days to an archive table — keep main table fast. |
| 67 | **Completion certificate/report** | Generate a per-booking completion report PDF with all details and timeline. |
| 68 | **BL release notification email** | Auto-send email to customer + sales person when BL is released. |
| 69 | **Completion analytics** | Show average time from booking creation to BL release, per line, per customer. |

---

## 🗃️ CATEGORY 6: MASTER DATA MANAGEMENT (12 Features)

| # | Feature | Description |
|---|---------|-------------|
| 70 | **Bulk import from Excel** | Upload an Excel file to bulk-import customers, ports, vessels, etc. into master data. |
| 71 | **Merge duplicates** | Detect near-duplicate master entries (e.g., "NHAVA SHEVA" vs "NHAVA SEVA") and merge them. |
| 72 | **Master data usage count** | Show how many bookings reference each master entry — prevent deleting entries in use. |
| 73 | **Master data deactivate (soft delete)** | Instead of hard delete, mark as inactive — hidden from dropdowns but preserved in existing bookings. |
| 74 | **Port database integration** | Pre-load a standard UN/LOCODE port database so users don't have to manually add every port. |
| 75 | **Vessel schedule integration** | Store vessel schedules per line — auto-suggest vessels based on POL + ETD. |
| 76 | **Customer credit limit** | Track customer payment status/credit limit — warn when booking for a customer with outstanding dues. |
| 77 | **Customer grouping** | Group customers by parent company (e.g., all Atul Limited divisions under one group). |
| 78 | **Sales person performance** | Track bookings per sales person — leaderboard on dashboard. |
| 79 | **Equipment type aliases** | Map aliases: "40HC" = "40'HC" = "40' HI-CUBE" = "40' Hi-Cube Container" — all resolve to same entry. |
| 80 | **Carrier contact directory** | Store carrier contacts (CS team email, DG desk, documentation) per line — quick access from entries. |
| 81 | **Master data changelog** | Track all changes to master data (who added/edited/deleted what, when). |

---

## 💰 CATEGORY 7: LOCALS / CHARGES (8 Features)

| # | Feature | Description |
|---|---------|-------------|
| 82 | **Rate history** | Keep history of rate changes — show previous rates with effective dates. |
| 83 | **Rate comparison** | Compare rates across lines for the same POL — help pick cheapest carrier. |
| 84 | **Rate expiry alerts** | Set validity dates on rates — alert when rates are about to expire. |
| 85 | **Freight calculator** | Given equipment type + POL + POD + Line, calculate total estimated freight from stored rates. |
| 86 | **Rate import from Excel** | Bulk import/update rates from carrier rate sheets (Excel files). |
| 87 | **Currency conversion** | Auto-convert between INR/USD/EUR using live or configurable exchange rates. |
| 88 | **Profit margin calculator** | Input selling rate vs buying rate — calculate margin per shipment. |
| 89 | **Rate negotiation tracker** | Track rate negotiation history with carriers — requested vs agreed rates. |

---

## 📧 CATEGORY 8: EMAIL & NOTIFICATIONS (10 Features)

| # | Feature | Description |
|---|---------|-------------|
| 90 | **SI filing email** | When SI Filed is checked, auto-send email to line's CS team with SI details. |
| 91 | **SOB notification email** | When Shipped on Board date is entered, email customer + sales person with SOB confirmation. |
| 92 | **Cut-off reminder emails** | Auto-send reminders 48h and 24h before port/SI cut-off dates. |
| 93 | **Daily pending summary email** | Scheduled daily email to operations team with pending SI/BL/DG counts. |
| 94 | **Email templates** | Configurable email templates per action (SI filing, BL release, SOB) with merge fields. |
| 95 | **Email log/history** | Store all sent emails with timestamps — viewable per booking entry. |
| 96 | **WhatsApp/SMS integration** | Send notifications via WhatsApp Business API or SMS for urgent cut-off reminders. |
| 97 | **CC/BCC configuration** | Per-customer or per-line email CC rules (e.g., always CC sales person for MAERSK bookings). |
| 98 | **Attachment support in emails** | Attach booking confirmation PDF, BL copy, or invoice when sending emails. |
| 99 | **Failed email retry queue** | If email fails (SMTP down), queue for retry — show failed emails for manual resend. |

---

## 🔍 CATEGORY 9: SEARCH & FILTERS (8 Features)

| # | Feature | Description |
|---|---------|-------------|
| 100 | **Global search** | Single search bar that searches across ALL tables (entries, completed, master data) simultaneously. |
| 101 | **Natural language search** | "Show me all CMA CGM bookings to Japan this month" → AI interprets and filters. |
| 102 | **Search history** | Remember last 10 searches — quick re-search. |
| 103 | **Saved searches/bookmarks** | Save complex search queries as named bookmarks. |
| 104 | **Date range filter on all grids** | Add date range filter (by ETD, booking date) on entries and completed files grids. |
| 105 | **Boolean field filter toggles** | Quick filter toggles: "Show only SI pending", "Show only HAZ", "Show only nominated". |
| 106 | **Cross-tab search** | Search in View Entries and see results from Completed Files too (combined view). |
| 107 | **Export filtered results** | Whatever filter is active, export those filtered rows to Excel (already partially done — enhance). |

---

## 📈 CATEGORY 10: REPORTING (10 Features)

| # | Feature | Description |
|---|---------|-------------|
| 108 | **Monthly booking report** | Auto-generated monthly PDF report: total bookings, by line, by customer, by route, with charts. |
| 109 | **Customer-wise statement** | Per-customer report: all their bookings in a period, status breakdown, pending items. |
| 110 | **Line-wise performance report** | Per-line report: volumes, on-time %, average transit time, issue frequency. |
| 111 | **Pending items report** | Consolidated report of all pending items across all stages — for daily operations review. |
| 112 | **DG cargo report** | All dangerous goods shipments: UN numbers, classes, vessels — for compliance tracking. |
| 113 | **Detention/demurrage risk report** | Flag bookings where containers have been out for >5 days without gate-in. |
| 114 | **Sales person commission report** | If commission tracking is added, generate per-sales-person commission reports. |
| 115 | **Custom report builder** | Drag-and-drop report builder — select fields, filters, grouping, chart type. |
| 116 | **Scheduled reports** | Configure reports to auto-generate and email weekly/monthly. |
| 117 | **Report sharing** | Share report links with view-only access (no login required). |

---

## 🔗 CATEGORY 11: INTEGRATIONS (10 Features)

| # | Feature | Description |
|---|---------|-------------|
| 118 | **Vessel tracking API** | Integrate with MarineTraffic/VesselFinder API to show real-time vessel positions on a map. |
| 119 | **Port schedule API** | Pull live vessel ETAs from port authority APIs (JNPT, Mundra port). |
| 120 | **Carrier API integration** | Connect to carrier APIs (Maersk Track & Trace, CMA CGM API) for real-time booking status updates. |
| 121 | **Customs/ICEGATE integration** | Pull shipping bill status from ICEGATE for Indian customs. |
| 122 | **Google Drive/OneDrive backup** | Auto-backup database and uploaded documents to cloud storage. |
| 123 | **Tally/accounting integration** | Push invoice data to Tally or other accounting software. |
| 124 | **WhatsApp Business API** | Send booking confirmations and status updates to customers via WhatsApp. |
| 125 | **Calendar integration** | Sync cut-off dates and ETDs to Google Calendar / Outlook Calendar. |
| 126 | **Slack/Teams notifications** | Send booking alerts to a Slack or Teams channel. |
| 127 | **Webhook support** | Fire webhooks on booking events (created, SI filed, BL released) for external system integration. |

---

## 🖥️ CATEGORY 12: UI/UX ENHANCEMENTS (15 Features)

| # | Feature | Description |
|---|---------|-------------|
| 128 | **Dark mode** | Toggle between light and dark themes — save preference. |
| 129 | **Responsive mobile layout** | Optimize all pages for mobile/tablet — collapsible sidebar, stacked cards, scrollable grids. |
| 130 | **Keyboard shortcuts** | Ctrl+N = New Booking, Ctrl+S = Save, Ctrl+F = Search, Escape = Close modal. |
| 131 | **Breadcrumb navigation** | Show current location: Home > Booking Portal > Add Booking. |
| 132 | **Loading skeletons** | Show skeleton loading animations instead of blank screens while data loads. |
| 133 | **Toast notification improvements** | Different toast styles for success/error/warning. Show undo option on delete toasts. |
| 134 | **Form validation indicators** | Real-time field validation — green checkmark for valid, red border for invalid. |
| 135 | **Tooltip help text** | Hover tooltips on form fields explaining what to enter (e.g., "Port Cut-off format: DD/MM-HHMM HRS"). |
| 136 | **Data table sticky header** | Grid column headers stay visible while scrolling down through many rows. |
| 137 | **Notification bell** | In-app notification center — show recent booking updates, cut-off reminders, system alerts. |
| 138 | **Drag-and-drop file upload** | Drag PDFs directly into the Add Booking form instead of clicking file input. |
| 139 | **Multi-language support** | UI translations for Hindi, Gujarati, Marathi — for operations staff convenience. |
| 140 | **Customizable sidebar** | Let users pin/unpin frequently used pages in the sidebar. |
| 141 | **Split-screen view** | View two pages side by side (e.g., Add Booking + View Entries). |
| 142 | **Onboarding tour** | First-time user walkthrough highlighting key features. |

---

## ⚙️ CATEGORY 13: SYSTEM & DATA (10 Features)

| # | Feature | Description |
|---|---------|-------------|
| 143 | **Audit trail per entry** | Track every field change with user, timestamp, old/new value — viewable in entry detail panel. |
| 144 | **Data backup/restore** | One-click database backup to local file. Restore from backup. |
| 145 | **Database migration tool** | Import data from old Firebase-based system into new PostgreSQL database. |
| 146 | **Soft delete / trash bin** | Deleted entries go to trash — recoverable within 30 days. |
| 147 | **Data validation rules** | Configurable business rules: "ETD cannot be more than 90 days in future", "Booking No must be unique". |
| 148 | **Duplicate entry finder** | Scheduled scan to find potential duplicate bookings across the database. |
| 149 | **Database indexing optimization** | Add proper indexes on frequently queried fields (booking_no, customer, etd, location). |
| 150 | **API rate limiting** | Prevent abuse with rate limiting on public-facing endpoints. |
| 151 | **Health monitoring dashboard** | System health page: DB connection status, API response times, email queue status. |
| 152 | **Configurable settings page** | In-app settings: SMTP config, API keys, default location, date format preference, auto-refresh interval. |

---

## 🏆 Priority Implementation Order

### Phase 1 — Immediate Impact (Features 1-5, 15-17, 33, 48-50, 68, 90-92, 143)
Core AI PDF import, dashboard KPIs, auto-fill, grid UX, email notifications, audit trail.

### Phase 2 — Operational Efficiency (Features 6-8, 34-37, 51-55, 70-73, 82-85, 100-105)
Multi-PDF, templates, bulk operations, master data tools, rate management, search.

### Phase 3 — Analytics & Reporting (Features 18-30, 108-117)
Advanced dashboards, custom reports, scheduled reports.

### Phase 4 — Integrations & Scale (Features 118-127, 144-152)
External APIs, backups, system optimizations.

---

## Environment Variables Reference

```bash
# Database
DATABASE_URL=postgresql://booking_user:booking_pass@localhost:5432/booking_portal

# Azure OpenAI (for AI PDF extraction)
AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com/
AZURE_OPENAI_KEY=your-key-here
AZURE_OPENAI_DEPLOYMENT=gpt-4o
AZURE_OPENAI_API_VERSION=2024-02-15-preview

# Email (SMTP Gmail)
SMTP_USER=tanks@dessertmarine.com
SMTP_PASS=awoukyetgbgsbtud
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587

# Optional: Azure Blob Storage (for PDF storage)
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;AccountName=...
AZURE_STORAGE_CONTAINER=booking-pdfs

# Optional: Vessel Tracking
MARINETRAFFIC_API_KEY=your-key

# Optional: WhatsApp
WHATSAPP_API_TOKEN=your-token
WHATSAPP_PHONE_ID=your-phone-id
```
