# 🧪 NEW FEATURES TESTING GUIDE — 31 Features

---

## How to Access
1. Start backend: `cd backend && python main.py`
2. Start frontend: `cd frontend && npm start`
3. Go to **Booking Portal** → **Analytics & KPIs** tab (new tab)
4. Or test APIs directly at http://localhost:8000/docs

---

## FEATURE 1: Shipment Conversion Rate KPI
**API**: `GET /api/analytics/kpis`
**UI**: Analytics tab → First KPI card (blue) → Shows "XX% Conversion Rate"
**Test**:
1. Create 10 bookings via Add Booking
2. Complete 6 of them (mark BL Released in View Entries)
3. Go to Analytics → ✅ Shows "60% Conversion Rate" with "6/10 completed"

## FEATURE 2: Average Booking-to-Sailing TAT
**API**: `GET /api/analytics/kpis` → `avg_booking_to_sailing_days`
**UI**: Analytics tab → Second KPI card (green)
**Test**:
1. Create booking with booking_date = 2026-04-01, ETD = 2026-04-10
2. Create another with booking_date = 2026-04-01, ETD = 2026-04-06
3. ✅ Shows "7d" average (9 + 5 / 2 = 7)

## FEATURE 3: BL Release Cycle Time
**API**: `GET /api/analytics/kpis` → `avg_bl_cycle_days`
**UI**: Analytics tab → Third KPI card (red)
**Test**:
1. Create booking, wait 3 days, complete it
2. ✅ Shows average days from creation to BL release

## FEATURE 4: SI Filing Timeliness KPI
**API**: `GET /api/analytics/kpis` → `si_timeliness_pct`
**UI**: Analytics tab → Fourth KPI card (yellow)
**Test**:
1. Create 10 bookings with si_cut_off set
2. Mark SI Filed on 8 of them
3. ✅ Shows "80% SI Filing Rate"

## FEATURE 5: Port Cut-off Compliance Rate
**API**: `GET /api/analytics/kpis` → `cutoff_compliance_pct`
**UI**: Analytics tab → Fifth KPI card (purple)
**Test**:
1. Create bookings with port_cut_off set
2. Add container numbers to some (indicating gate-in met cutoff)
3. ✅ Shows compliance percentage

## FEATURE 6: Line-wise Performance Scorecard
**API**: `GET /api/analytics/line-scorecard`
**UI**: Analytics tab → "🚢 Line Performance Scorecard" table
**Test**:
1. Create bookings across CMA CGM, MAERSK, ARKAS
2. Complete some, leave others active
3. ✅ Table shows per-line: total, completed, conversion %, correction %, avg BL days, SI %
4. Click 📥 → ✅ Excel downloads
5. Click 📧 → ✅ Email dialog → sends table

## FEATURE 7: Customer Revenue Concentration
**API**: `GET /api/analytics/customer-concentration`
**UI**: Analytics tab → "👥 Customer Concentration" table
**Test**:
1. Create 20 bookings: 10 for "CUSTOMER A", 5 for "CUSTOMER B", 5 for "CUSTOMER C"
2. ✅ Table shows Customer A = 50%, B = 25%, C = 25%

## FEATURE 8: Monthly/Weekly Trend Comparison
**API**: `GET /api/analytics/trend-comparison`
**UI**: Analytics tab → "📈 Monthly Booking Trend" line chart
**Test**:
1. Create bookings with different booking_dates across months
2. ✅ Line chart shows monthly booking count trend

## FEATURE 9: POD/FPOD Destination Heatmap
**API**: `GET /api/analytics/destination-heatmap`
**UI**: Analytics tab → "🌍 Top Destinations" horizontal bar chart
**Test**:
1. Create bookings to DALIAN, SINGAPORE, HOUSTON, KOBE
2. ✅ Bar chart shows destinations sorted by booking count

## FEATURE 10: Equipment Utilization Dashboard
**API**: `GET /api/analytics/equipment-utilization`
**UI**: Analytics tab → "📦 Equipment Utilization" pie chart
**Test**:
1. Create bookings with 40'HC, 20'DV, 20'TK
2. ✅ Pie chart shows distribution of container types

## FEATURE 11: Sales Person Leaderboard
**API**: `GET /api/analytics/sales-leaderboard`
**UI**: Analytics tab → "🏆 Sales Person Leaderboard" table with 🥇🥈🥉
**Test**:
1. Create bookings with different sales_person_name values
2. ✅ Table shows ranking by total bookings with medals for top 3

## FEATURE 12: Vessel/Voyage Load Factor
**API**: `GET /api/analytics/vessel-load`
**UI**: Analytics tab → "🚢 Vessel/Voyage Load Factor" table
**Test**:
1. Create multiple bookings on same vessel/voyage
2. ✅ Table shows booking count and container count per vessel

## FEATURE 13: Booking Validity Expiry Alerts
**API**: `GET /api/analytics/validity-alerts`
**UI**: Analytics tab → "⚠️ Booking Validity Expiry Alerts" (red border table, only shows if alerts exist)
**Test**:
1. Create booking with booking_validity = yesterday → ✅ Shows "EXPIRED" in red
2. Create booking with booking_validity = today → ✅ Shows "TODAY" in yellow
3. Create booking with booking_validity = 2 days from now → ✅ Shows "2d left"

## FEATURE 14: Corrections Rate per Line/Customer
**API**: `GET /api/analytics/corrections-by-line`
**UI**: Data available via API (used in Line Scorecard table)
**Test at** http://localhost:8000/docs → GET /api/analytics/corrections-by-line
✅ Returns `by_line` and `by_customer` arrays with correction rates

## FEATURE 15-17: Scheduled/Customer-Specific/Sales Person Emails
**API**: `POST /api/ai/generate-email`
**Test**:
1. At http://localhost:8000/docs → POST /api/ai/generate-email
2. Set `email_type` = "customer_status", `entry_id` = (any booking ID)
3. ✅ Returns AI-generated email with subject, body, to, cc
4. Try types: "si_reminder", "bl_release", "cutoff_reminder"
5. ⚠️ Requires AZURE_OPENAI keys in .env

## FEATURE 18: Line-Specific Booking Summary Email
**Test**:
1. Go to Analytics → Line Scorecard
2. Click 📧 → enter email → ✅ Sends line-wise performance email

## FEATURE 19: AI-Powered PDF Report (via AI Summary)
**API**: `POST /api/ai/smart-summary`
**UI**: Analytics tab → "🤖 AI Summary" button (top right)
**Test**:
1. Click "🤖 AI Summary" button
2. If Azure configured → ✅ Shows AI briefing with priorities, risks, positives
3. If not configured → ✅ Shows "AI not configured" message

## FEATURE 20: Data Export with Custom Column Selection
**Test**: Every table in Analytics has 📥 Excel button → downloads that specific table's data

## FEATURE 21-22: Locals Rate Comparison
**API**: `GET /api/analytics/locals-comparison?charge_name=THC`
**Test at** http://localhost:8000/docs:
1. First save some Locals charges (Locals tab)
2. Call API with charge_name=THC → ✅ Returns comparison across all lines/POLs
3. Try charge_name=DOC, SEAL, etc.

## FEATURE 23: Historical Rate Trend
**API**: `GET /api/rate-history?line=CMA CGM&pol=NHAVA SHEVA`
**Test**: Rate history table stores changes (currently saves when locals are updated — needs rate_history writes added to locals save endpoint for full functionality)

## FEATURE 24: Customer Booking Pattern Analysis
**API**: `GET /api/analytics/customer-patterns/{customer_name}`
**Test**:
1. At http://localhost:8000/docs → GET /api/analytics/customer-patterns/DESSERT MARINE SERVICES (I) PVT LTD
2. ✅ Returns: top routes, equipment preferences, monthly trend, preferred lines

## FEATURE 25: Duplicate Booking Detection
**API**: `GET /api/analytics/duplicate-detection`
**UI**: Analytics tab → "🔍 Possible Duplicate Bookings" (shows only if duplicates found)
**Test**:
1. Create 2 bookings with same customer + same vessel + same POD + same ETD but different booking numbers
2. Go to Analytics → ✅ Red-bordered table shows the pair

## FEATURE 26: Shipment Milestone Timeline
**API**: `GET /api/analytics/milestone-timeline/{entry_id}`
**Test at** http://localhost:8000/docs:
1. Call with any entry ID
2. ✅ Returns ordered milestones: Created → VGM → SI → DG → Print → Corrections → BL
3. Each shows done=true/false

## FEATURE 27: Custom Dashboard Widget Builder
**Current**: Analytics page IS the widget dashboard — shows all KPIs, charts, tables in one scrollable view. Each section can export independently.

## FEATURE 28: Data Audit Trail
**API**: `GET /api/audit/{entry_id}`, `POST /api/audit`
**Test**:
1. POST /api/audit with entry_id=1, field="customer", old_value="OLD", new_value="NEW"
2. GET /api/audit/1 → ✅ Returns change history

## FEATURE 29: Cross-Location Performance
**API**: `GET /api/analytics/cross-location`
**UI**: Analytics tab → "📍 Cross-Location Comparison" table
**Test**:
1. Create bookings in MUMBAI and GUJARAT locations
2. ✅ Table shows side-by-side: total, active, completed, pending SI, pending BL, correction %, avg BL days

## FEATURE 30: AI Booking Analysis
**API**: `POST /api/ai/analyze-booking/{entry_id}`
**Test at** http://localhost:8000/docs:
1. Call with any entry ID
2. If Azure configured → ✅ Returns risk_alerts, suggestions, estimated_transit_days, compliance_notes
3. ⚠️ Requires AZURE_OPENAI keys in .env

## FEATURE 31: Profit/Loss Estimation
**Current**: Locals charges + equipment data exists. The analytics endpoints provide the data foundation. Full P&L calculation needs a selling_rate field addition (future enhancement).

---

## QUICK SMOKE TEST (All 31 features in 5 minutes)

```
1. Start backend + frontend
2. Create 5-10 bookings with different lines, customers, POLs, PODs
3. Complete 3-4 of them (BL Released)
4. Go to Analytics & KPIs tab
5. ✅ 9 KPI cards show values
6. ✅ Monthly trend line chart shows data
7. ✅ Equipment pie chart shows distribution
8. ✅ Destination bar chart shows top ports
9. ✅ Line Scorecard table has rows
10. ✅ Customer Concentration table has rows
11. ✅ Sales Leaderboard has rows
12. ✅ Cross-Location table compares locations
13. ✅ Vessel Load table shows vessel data
14. Click 📥 on any table → ✅ Excel downloads
15. Click 📧 on any table → ✅ Email dialog works
16. Click "🤖 AI Summary" → ✅ Shows result (or "not configured")
17. Create 2 bookings with same customer+vessel+pod+etd → ✅ Duplicate alert appears
18. Create booking with expired validity → ✅ Validity alert appears
19. Open http://localhost:8000/docs → Test API endpoints directly
20. Done!
```

---

## Azure OpenAI Features (require .env keys)

| Feature | API | Works without Azure? |
|---------|-----|---------------------|
| AI PDF Extraction | POST /api/parse-pdf | ✅ Falls back to regex |
| AI Booking Analysis | POST /api/ai/analyze-booking/{id} | ❌ Returns "not configured" error |
| AI Email Generation | POST /api/ai/generate-email | ❌ Returns "not configured" error |
| AI Daily Summary | POST /api/ai/smart-summary | ❌ Shows fallback message in UI |

Everything else works without Azure — it only enhances PDF parsing and adds AI insights.
