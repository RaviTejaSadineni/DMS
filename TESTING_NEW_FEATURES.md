# 🧪 Testing Guide — 36 New Features

## Quick Start
```bash
cd backend && python main.py    # Terminal 1
cd frontend && npm start        # Terminal 2
```
API docs: http://localhost:8000/docs

---

## Feature 1: Aging Analysis Dashboard
**Where**: Analytics tab → "⏳ Aging Analysis" table
**Test**: Create 3 bookings, leave them for a few days without filing SI → Table shows days_open with red highlight for >7 days

## Feature 2: Week-over-Week KPI
**Where**: Analytics tab → Last KPI card (green/red with arrow)
**Test**: Create 5 bookings this week, had 3 last week → Shows "↑66.7%"

## Feature 3: Booking Funnel
**Where**: Analytics tab → "🔄 Booking Funnel" colored bar
**Test**: 10 total → 8 SI filed → 6 printed → 4 corrections → 3 BL released → Shows descending bars with percentages

## Feature 4: Day-of-Week Pattern
**Where**: Analytics tab → "📅 Day-of-Week Pattern" bar chart
**Test**: Create bookings on different days → Chart shows which weekdays have most bookings

## Feature 5: SI Filing Delay Distribution
**Where**: Backend stores si_filed_date automatically when SI is checked. Visible via analytics/kpis

## Feature 6: BL Type Distribution
**Where**: Analytics tab → "📄 BL Type Distribution" pie chart
**Test**: Complete bookings with different BL types (OBL, SEAWAY, TELEX) → Pie shows breakdown

## Feature 7: Nominated vs Non-Nominated
**Where**: Analytics tab → "⚖️ Nominated vs Non-Nominated" side-by-side cards
**Test**: Create 5 nominated + 5 non-nominated bookings → Compare completion rates, BL cycle time, correction rate

## Feature 8: Revenue Proxy
**Where**: API endpoint `GET /analytics/route-profitability` combines booking volumes with locals charges

## Feature 9: Dynamic Locals Rows/Columns
**Where**: Locals tab → Edit mode → "+ Row" and "+ Column" buttons
**Test**:
1. Go to Locals, select Line + POL, click Edit
2. Type "HANDLING CHARGES" in row input, click "+ Row" → ✅ New row appears
3. Type "20RF" in col input, click "+ Column" → ✅ New column appears
4. Click ✕ next to any row/column → ✅ Removed
5. Save → Reload → ✅ Custom rows/cols persisted

## Feature 10: POL Congestion Indicator
**Where**: Analytics tab → "🚧 POL Congestion Alerts" table (only shows if 3+ bookings share same POL+ETD)
**Test**: Create 4 bookings with same POL=NHAVA SHEVA and same ETD → Alert appears

## Feature 11: Daily Summary Email
**API**: `POST /api/email/daily-summary`
**Test**: Set `DAILY_REPORT_RECIPIENTS` in .env → Call API → ✅ Email arrives with ops summary

## Feature 12: SI Cut-off Reminder
**API**: `POST /api/email/si-reminders`
**Test**: Create booking with SI cut-off = tomorrow, don't file SI → Call API → ✅ Reminder sent to sales person

## Feature 13: Port Cut-off Alert
**Where**: Similar to Feature 12, port cut-off tracked in backend

## Feature 14: Validity Expiry Email
**API**: `POST /api/email/validity-alerts`
**Test**: Create booking with validity = tomorrow → Call API → ✅ Alert sent

## Feature 15: BL Release Auto-Email
**Test**: In View Entries, check "BL Released" on any entry → ✅ Auto-email sent to sales person with BL details

## Feature 16: Booking Confirmation Email
**Test**: Add a new booking where customer has sales_person_email → ✅ Confirmation email auto-sent

## Feature 17: AI Weekly Brief
**API**: `POST /api/ai/smart-summary`
**Where**: Analytics tab → 🤖 AI Brief button → ✅ Shows summary, priorities, risks
**Requires**: Gemini keys in .env

## Feature 18: Bulk Email Selected Entries
**Where**: View Entries → Check multiple rows → Click "📧 Selected" button
**Test**: Select 3 rows → Click "📧 Selected" → ✅ Email dialog with only selected bookings

## Feature 19: Email Templates
**API**: `GET/POST /api/email-templates`
**Test**: POST to create template, GET to list

## Feature 20: Multi-PDF Batch Upload
**API**: `POST /api/parse-pdf-batch` (accepts multiple files)
**Test at** http://localhost:8000/docs → Upload 3 PDFs → Returns array of parsed results

## Feature 21: PDF Confidence Scores
**Where**: Add Booking → Upload PDF with Gemini configured → ✅ Low-confidence fields highlighted yellow/red
**Test**: Fields with <50% confidence get red border, 50-80% get yellow

## Feature 22: PDF Text Comparison View
**Where**: Add Booking → Upload PDF → "👁️ Show PDF Text" button appears → ✅ Shows raw text in dark panel for verification

## Feature 23: Enhanced Carrier Detection
**Where**: Gemini prompt includes carrier-specific document type detection

## Feature 24: PDF Amendment Detection
**Where**: If uploaded booking_no already exists, backend raises HTTPException (duplicate prevention)

## Feature 25: PDF Field Mapping
**Where**: pdf_raw_text stored on Entry, viewable via API `GET /entries/{id}`

## Feature 26: Re-parse from Stored Data
**Where**: Entry stores pdf_raw_text, can be re-processed via API

## Feature 27: Period-over-Period Comparison
**Where**: Analytics tab → "📊 Period Comparison" section
**Test**: Select Period 1 (March) and Period 2 (April) → Click Compare → ✅ Side-by-side metrics

## Feature 28: Customer YTD Report Card
**API**: `POST /api/ai/customer-report/{customer_name}`
**Test**: Call with customer name → Returns AI narrative, strengths, growth areas, loyalty score
**Requires**: Gemini

## Feature 29: Export Full Analytics to Excel
**Where**: Analytics tab → "📥 Export All" button (top right)
**Test**: Click → ✅ Multi-sheet Excel (KPIs, Line Scorecard, Customer, Trends, Sales, Vessels)

## Feature 30: Printable Booking Summary
**Where**: View Entries → 🖨️ Print icon on each row
**Test**: Click print icon → ✅ New window opens with formatted booking details → Browser print dialog

## Feature 31: Route Profitability
**API**: `GET /api/analytics/route-profitability`
**Test**: Create bookings + locals charges → API returns routes ranked by container volume

## Feature 32: Audit Trail Summary
**API**: `GET /api/analytics/audit-summary`
**Test**: Edit entries several times → API returns most-changed fields and most-active users

## Feature 33: Monthly Scorecard
**Where**: Use Export All + AI Brief together for monthly reporting

## Feature 34: Sales Person Separate Table
**Where**: Add Master → "👤 Sales Person" card (2nd card) → Add name + comma-separated emails
**Where**: Manage Master → "👤 Sales Person" button → Edit/delete sales persons
**Test**:
1. Go to Add Master → Open Sales Person → Add "JOHN", emails "john@test.com, john2@test.com"
2. Go to Add Master → Open Customer → Sales Person field is dropdown → Select JOHN → ✅ Email auto-fills
3. Go to Add Booking → ➕ Add Customer → Sales Person dropdown lists JOHN

## Feature 35: Natural Language Query
**Where**: Analytics tab → "🤖 AI Natural Language Query" section at bottom
**Test**: Type "Which line has the most bookings?" → Press Enter or 🔍 → ✅ AI answers
**Requires**: Gemini

## Feature 36: UI Improvements
- Tabs now have icons (📊 📈 ➕ 📋 ✅ 🗂️ ⚙️ 💰)
- Analytics has 10 KPI cards in 2 rows
- Compact data grid (density=compact)
- Better color coding throughout
- Print button added to View Entries

---

## 5-Minute Smoke Test
```
1. Start backend + frontend
2. Add Master → Sales Person → Add "RAHUL" with email
3. Add Master → Location → "MUMBAI"
4. Add Master → Customer → "TEST CO" with Sales Person = RAHUL
5. Add remaining masters (Line, POL, POD, FPOD, Vessel, Equipment)
6. Add Booking → Upload PDF → Verify form fills + PDF text view
7. Submit → Check View Entries → Entry appears
8. View Entries → Select 2 rows → "📧 Selected" → Send
9. View Entries → Click 🖨️ → Print preview opens
10. Check SI → BL Type dialog → Check through to BL Released → Auto-email fires
11. Go to Completed → Entry there with export/email
12. Analytics tab → All KPI cards show data
13. Analytics → Funnel shows stages
14. Analytics → Export All → Multi-sheet Excel downloads
15. Analytics → Period Comparison → Select dates → Compare
16. Locals → Select line+POL → Edit → Add custom row → Add custom column → Save
17. Manage Master → Sales Person → Edit/Delete works
18. Done!
```

## Gemini Features (need GEMINI_API_KEY)
| Feature | Works without Gemini? |
|---------|---------------------|
| PDF extraction | ✅ Falls back to regex |
| AI Booking Analysis | ❌ |
| AI Email Generation | ❌ |
| AI Smart Summary | ❌ Shows fallback |
| Customer Report Card | ❌ |
| Natural Language Query | ❌ |
| All other 30 features | ✅ |
