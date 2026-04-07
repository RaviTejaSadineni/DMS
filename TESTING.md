# 🧪 TESTING GUIDE — All Implemented Features

**Total: 67 testable features across 10 areas**

Prerequisites: Backend running on http://localhost:8000, Frontend on http://localhost:3000

---

## AREA 1: NAVIGATION & LAYOUT (3 features)

### Feature 1.1: Sidebar Navigation
**What**: Fixed left sidebar with Home and Booking Portal tabs
**Test**:
1. Open http://localhost:3000
2. ✅ Sidebar visible on left with "DMS Portal" header
3. Click "Home" → shows "Hi, Welcome!" page
4. Click "Booking Portal" → shows tabbed interface
5. ✅ Active tab is highlighted with blue border

### Feature 1.2: Tabbed Booking Portal
**What**: 7 tabs inside Booking Portal — Dashboard, Add Booking, View Entries, Completed Files, Add Master, Manage Master, Locals
**Test**:
1. Click "Booking Portal" in sidebar
2. ✅ 7 tabs visible in top bar
3. Click each tab → correct page loads
4. ✅ Active tab has blue underline

### Feature 1.3: Full-screen Tab View
**What**: Booking Portal tabs take full remaining screen width
**Test**:
1. Navigate to Booking Portal
2. ✅ Content fills full width (sidebar + content = full screen)
3. ✅ Content area scrolls independently

---

## AREA 2: DASHBOARD (10 features)

### Feature 2.1: Location Filter Buttons (from backend)
**What**: Location buttons come from database, not hardcoded
**Test**:
1. Go to Dashboard tab
2. ✅ "All" button always visible
3. Add locations via Add Master (e.g., "MUMBAI", "GUJARAT")
4. Refresh Dashboard → ✅ MUMBAI and GUJARAT buttons appear
5. Click MUMBAI → ✅ Only MUMBAI entries in metrics
6. Click All → ✅ All entries shown

### Feature 2.2: 7 Metric Cards
**What**: Pending SI, First Print, Correction, BL, Invoice, DG, Empty Pickup
**Test**:
1. Create a booking entry (via Add Booking)
2. ✅ "PENDING SI" count increases by 1
3. In View Entries, check SI Filed → ✅ PENDING SI decreases, PENDING FIRST PRINT increases
4. Check First Printed → ✅ PENDING FIRST PRINT decreases, PENDING CORRECTION increases
5. Continue through corrections → BL released → ✅ counts shift correctly
6. Create a booking with no container number → ✅ EMPTY PICKUP count increases

### Feature 2.3: Clickable Metric Cards → Filtered View
**What**: Click any card to see the actual entries
**Test**:
1. Click "PENDING SI" card
2. ✅ Modal opens showing table of entries where SI is not filed
3. ✅ Count in modal header matches card number
4. Click Close → modal closes

### Feature 2.4: Date Range Filter
**What**: Filter dashboard by ETD date range
**Test**:
1. Set start date to 2026-03-01, end date to 2026-03-31
2. ✅ Bar chart only shows entries with ETD in March
3. Click "Reset" → ✅ Resets to last 7 days

### Feature 2.5: ETD Bar Chart
**What**: Shipments grouped by sailing date
**Test**:
1. Create 3 entries with ETD = today, 2 entries with ETD = tomorrow
2. Refresh dashboard → ✅ Bar chart shows 2 bars (today: 3, tomorrow: 2)

### Feature 2.6: Export Filtered View to Excel
**What**: Export button in filtered modal
**Test**:
1. Click any metric card to open filtered view
2. Click "📥 Excel" button
3. ✅ .xlsx file downloads
4. Open in Excel → ✅ Contains all shown entries with correct columns

### Feature 2.7: Share Filtered View via Email
**What**: Email button sends HTML table to recipient
**Test**:
1. Click any metric card to open filtered view
2. Click "📧 Email" button
3. ✅ Email dialog opens asking for recipient email
4. Enter your email, click Send
5. ✅ Toast shows "Email sent!"
6. Check your inbox → ✅ Email received with HTML table of entries

### Feature 2.8: Auto-refresh on Filter Change
**What**: Dashboard data reloads when location or date changes
**Test**:
1. Change location from All to MUMBAI
2. ✅ All numbers update immediately (no manual refresh needed)

### Feature 2.9: Card Hover Animation
**What**: Cards lift up on hover
**Test**:
1. Hover over any metric card
2. ✅ Card moves up slightly (translateY animation)

### Feature 2.10: Filtered View Close
**What**: Close button dismisses the filtered modal
**Test**:
1. Open filtered view
2. Click "✕ Close" → ✅ Modal closes
3. Dashboard remains in same state

---

## AREA 3: ADD BOOKING (14 features)

### Feature 3.1: PDF Import — Auto-fill Form
**What**: Upload carrier PDF → form fields auto-populated
**Test**:
1. Go to Add Booking tab
2. Click file input next to "Import from PDF"
3. Upload an Arkas booking PDF
4. ✅ Booking No = "NSA090410707"
5. ✅ Customer = "ULTRA CHEMICAL WORKS"
6. ✅ Line = "ARKAS LINE"
7. ✅ Vessel = "SASKIA A", Voyage = "IMS12W26"
8. ✅ POL = "NHAVA SHEVA"
9. ✅ POD = "ALEXANDRIA OLD PORT"
10. ✅ FPOD = "CASABLANCA"
11. ✅ Equipment = 1 x 40'HC
12. ✅ Location = "MUMBAI" (auto-detected)

### Feature 3.2: PDF Import — Auto-add to Master Dropdowns
**What**: Values from PDF are auto-created in master data if they don't exist
**Test**:
1. Start with empty database (no master data)
2. Upload a PDF with vessel "SASKIA A"
3. ✅ Toast says "PDF imported & dropdowns updated!"
4. ✅ "SASKIA A" now appears in the Vessel dropdown
5. Go to Manage Master → Vessels → ✅ "SASKIA A" is listed
6. Upload another booking with same vessel → ✅ No duplicate created

### Feature 3.3: PDF Import — Multi-carrier Support
**What**: Parser handles 20+ carrier formats
**Test with each carrier**:
- Arkas: Booking = NSA..., Vessel/Voyage combined
- CMA CGM: Booking Number = AMC..., ETD format "08-APR-2026 02:30"
- COSCO: BOOKING NUMBER = COAU..., PLACE OF RECEIPT vs PORT OF LOADING
- Diamond Maritime: Portal Booking Ref, vessel in separate field
- ESL: Book No = ESLINDMUN..., Load Port ETD
- Evergreen: BOOKING NO = 100680..., VESSEL/VOYAGE format
- Hapag-Lloyd: Our Reference = 39779900, equipment in table
- HMM: Booking Number = BOME..., 1st Vessel/Voyage
- Interasia: Book No = A32GX..., POL/POD/PLD fields
- Maersk: Booking No = 268987135, From/To fields
- MSC: BOOKING REFERENCE = EBKG..., VESSEL NAME / FLAG
- Muskan: BOOKING REF = MUSKAN306, VOLUME field
- ONE: Booking No = MUMG..., Trunk Vessel
- Samudera: BOOKING CONFIRMATION reference, LOADING VSL / VOY
- SCI: SR. NO in header, vessel in text
- Sinokor: B/L No as booking ref, VSL Name / Voy
- Star Shipping: Booking Ref = GOSUBOM..., VESSEL + ETD
- TransLiner: Booking No = TRLNSASHA..., Vessel / Voyage
- Wan Hai: Booking No = 067GX..., Estimated Departure Date
- Transorient (SNL): SR. NO as booking ref

### Feature 3.4: PDF Attachment Badge
**What**: Uploaded PDF name shown as removable badge
**Test**:
1. Upload a PDF
2. ✅ "📎 filename.pdf" badge appears with ✕ button
3. Click ✕ → ✅ Badge removed, file input cleared
4. Upload a different PDF → ✅ New badge shown, form re-filled

### Feature 3.5: Dropdown with Add New Option
**What**: Every dropdown has "➕ Add New" at top
**Test**:
1. Click Location dropdown
2. ✅ First option is "➕ Add New"
3. Click it → ✅ Modal opens with form fields for that master type
4. Fill name, click Save → ✅ New value appears in dropdown and is selected

### Feature 3.6: Dropdown with Remove Option
**What**: When a value is selected, dropdown shows "🗑️ Remove" option
**Test**:
1. Select "MUMBAI" from Location dropdown
2. Open dropdown again
3. ✅ Last option is '🗑️ Remove "MUMBAI"' (red text)
4. Click it → ✅ Confirm dialog appears
5. Confirm → ✅ "MUMBAI" removed from dropdown AND from database
6. Go to Manage Master → Locations → ✅ "MUMBAI" is gone

### Feature 3.7: Sales Person Linked Dropdown (in Add Customer modal)
**What**: When adding a customer, Sales Person is a dropdown that auto-fills email
**Test**:
1. First, add a customer with Sales Person = "JOHN" and email = "john@test.com"
2. Now click ➕ Add New on Customer dropdown
3. ✅ "Sales Person" field is a dropdown (not text input)
4. Select "JOHN" from it
5. ✅ "Sales Person Email" field auto-fills with "john@test.com"

### Feature 3.8: Equipment Details — Add/Remove
**What**: Add multiple equipment rows, each with type + qty + container no
**Test**:
1. Click "➕ Add" in Equipment section
2. ✅ Equipment row appears with Type dropdown, Qty input, Container No input
3. Add second row → ✅ Two rows visible
4. Click ✕ on first row → ✅ First row removed, second remains

### Feature 3.9: Cut-off Auto-format
**What**: Typing "06041800" auto-formats to "06/04-1800 HRS"
**Test**:
1. In Port Cut-off field, type "06041800"
2. ✅ Field shows "06/04-1800 HRS"
3. Type invalid "99991800" → ✅ Toast error "Invalid format"

### Feature 3.10: Booking No Duplicate Check
**What**: Backend rejects duplicate booking numbers
**Test**:
1. Add a booking with Booking No = "TEST001"
2. Try adding another with same Booking No
3. ✅ Toast error "Booking No already exists"

### Feature 3.11: Required Field Validation
**What**: Submit blocked if required fields empty
**Test**:
1. Leave all fields empty, click "Add Booking"
2. ✅ Toast error "Please fill all required fields!"
3. Required fields: Location, Customer, Line, POL, POD, FPOD, Vessel, Booking No, Equipment

### Feature 3.12: Nominated Checkbox
**What**: Mark booking as nominated shipment
**Test**:
1. Check "Nominated" checkbox next to Location
2. Submit booking
3. Go to View Entries → ✅ Entry has is_nominated = true

### Feature 3.13: Volume Auto-calculation
**What**: Volume string built from equipment details
**Test**:
1. Add equipment: Type = 40'HC, Qty = 2
2. Add second: Type = 20'DV, Qty = 1
3. Submit → ✅ Volume saved as "2 x 40'HC, 1 x 20'DV"

### Feature 3.14: Uppercase Enforcement
**What**: All text fields auto-converted to uppercase
**Test**:
1. Type "nhava sheva" in any field
2. ✅ Immediately shows "NHAVA SHEVA"

---

## AREA 4: VIEW ENTRIES (14 features)

### Feature 4.1: Location Filter Buttons (from backend)
**Test**: Same as Dashboard 2.1 — buttons come from database entries

### Feature 4.2: Full-text Search
**Test**:
1. Type "CMA" in search box
2. ✅ Only entries with "CMA" in any field shown
3. Type "40HC" → ✅ Only entries with that volume shown
4. Clear search → ✅ All entries return

### Feature 4.3: MUI DataGrid Inline Editing
**Test**:
1. Double-click on Customer cell
2. ✅ Cell becomes editable
3. Change value, press Enter
4. ✅ Toast "Updated!" — value saved to database
5. Refresh page → ✅ New value persists

### Feature 4.4: Checkbox Toggle — VGM, SI, DG, First Print, Corrections, Invoice
**Test**:
1. Find an entry, click VGM checkbox
2. ✅ Checkbox toggles to checked
3. ✅ Database updated (verify in Manage Master or API docs)
4. Refresh → ✅ Checkbox still checked

### Feature 4.5: SI Filed → BL Type Dialog
**Test**:
1. Find entry where SI Filed is unchecked
2. Click SI checkbox
3. ✅ Dialog opens: "Select BL Type" with buttons OBL, SEAWAY BL, EXPRESS BL, TELEX
4. Select "OBL", click Save
5. ✅ SI Filed = checked, BL Type = "OBL"

### Feature 4.6: BL Released → Move to Completed
**Test**:
1. Find entry with corrections finalised
2. Click BL Released checkbox
3. ✅ Confirm dialog: "Mark as BL Released? Moves to Completed."
4. Confirm → ✅ Entry disappears from View Entries
5. Go to Completed Files → ✅ Entry appears there

### Feature 4.7: Delete Entry
**Test**:
1. Click 🗑️ icon on any row
2. ✅ Dialog: "Delete booking XXX?"
3. Click Delete → ✅ Entry removed
4. ✅ Toast "Deleted!"

### Feature 4.8: Duplicate Entry
**Test**:
1. Click 📋 (copy) icon on any row
2. ✅ Dialog asking for new Booking No and Container No
3. Enter "DUP001", click Duplicate
4. ✅ New entry created with same data but new booking number
5. ✅ All status fields reset (SI=false, BL=false, etc.)

### Feature 4.9: Export to Excel
**Test**:
1. Click "📥 Excel" button
2. ✅ .xlsx file downloads with filename "entries.xlsx"
3. Open → ✅ All visible (filtered) entries with correct columns

### Feature 4.10: Share via Email
**Test**:
1. Click "📧 Email" button
2. ✅ Email dialog opens
3. Enter recipient email
4. Click Send → ✅ Email arrives with HTML table of entries

### Feature 4.11: Editable Columns
**Test**: Double-click these columns to verify they're editable:
- Customer ✅, Booking No ✅, Container No ✅, Volume ✅, Vessel ✅, Voyage ✅, BL Type ✅, BL No ✅, Remarks ✅

### Feature 4.12: Non-editable Columns
**Test**: Double-click these — they should NOT be editable:
- Location, Sales, Booking Date, Validity, Line, POL, POD, FPOD, Port CutOff, SI CutOff, ETD, all checkboxes

### Feature 4.13: Grid Density
**Test**: ✅ Grid renders in "compact" density (smaller row height for more data)

### Feature 4.14: Pagination
**Test**:
1. Add 30+ entries
2. ✅ Page size selector shows 25/50/100
3. Navigate between pages ✅

---

## AREA 5: COMPLETED FILES (5 features)

### Feature 5.1: Shows Only BL Released Entries
**Test**:
1. Release BL on an entry in View Entries
2. Go to Completed Files → ✅ Entry appears here
3. ✅ Entry NOT in View Entries anymore

### Feature 5.2: Read-only Grid
**Test**:
1. Try double-clicking any cell
2. ✅ Cell does NOT become editable
3. ✅ Checkboxes are disabled (greyed out)

### Feature 5.3: Location Filter (from backend)
**Test**: Same as View Entries — buttons from database

### Feature 5.4: Export to Excel
**Test**: Click "📥 Excel" → ✅ Downloads completed_files.xlsx

### Feature 5.5: Share via Email
**Test**: Click "📧 Email" → ✅ Dialog → Send → Email received

---

## AREA 6: ADD MASTER (8 features — one per category)

### Feature 6.1-6.8: Add Location / Customer / Line / POL / POD / FPOD / Vessel / Equipment Type
**Test for each**:
1. Go to Add Master tab
2. Click the category card (e.g., "📍 Location") → ✅ Card expands
3. Fill required fields (marked with *)
4. Click "✅ Add" → ✅ Toast "location added!"
5. Go to Add Booking → ✅ New value appears in dropdown
6. Go to Manage Master → ✅ New value appears in table

### Feature 6.9: Sales Person Dropdown in Customer Card
**Test**:
1. Open Customer card
2. ✅ "Sales Person" field is a dropdown
3. If existing sales persons exist, they appear as options
4. Select one → ✅ "Sales Person Email" auto-fills

### Feature 6.10: Accordion UI
**Test**:
1. Click Location card → ✅ Opens
2. Click Line card → ✅ Line opens, Location closes
3. Click Line again → ✅ Line closes

---

## AREA 7: MANAGE MASTER (8 features)

### Feature 7.1: Master Category Selector
**Test**: Click each button (Location, Customer, Line, POL, POD, FPOD, Vessel, Equipment Type) → ✅ Correct table loads

### Feature 7.2: Table Display
**Test**:
1. Select Customer → ✅ Table shows all columns: Name, Contact Person, Email, etc.
2. Select POL → ✅ Table shows single column: Name

### Feature 7.3: Inline Edit
**Test**:
1. Click "Edit" on any row
2. ✅ All cells become input fields
3. Change a value, click "Save"
4. ✅ Toast "Updated!" — value persisted

### Feature 7.4: Cancel Edit
**Test**:
1. Click Edit, change a value
2. Click "Cancel" → ✅ Original value restored

### Feature 7.5: Delete
**Test**:
1. Click "Delete" on any row
2. ✅ Confirm dialog
3. Confirm → ✅ Row removed from table and database

### Feature 7.6: Add New (inline)
**Test**:
1. Click "➕ Add" button
2. ✅ Input row appears at top
3. Fill values, click "Save" → ✅ New row added

### Feature 7.7: Search
**Test**:
1. Type in search box → ✅ Table filters in real-time
2. Clear search → ✅ All rows return

### Feature 7.8: Export to Excel
**Test**: Click "📥 Excel" → ✅ Downloads [Category].xlsx with all rows

---

## AREA 8: LOCALS CHARGES (6 features)

### Feature 8.1: Line + POL Selector
**Test**:
1. Go to Locals tab
2. ✅ Line dropdown populated from database
3. ✅ POL dropdown populated from database
4. Select both → ✅ Charge grid appears

### Feature 8.2: Charge Grid Display
**Test**: ✅ 11 charge rows (THC, DOC, SEAL, etc.) × 4 equipment columns (20 DV, 40DV, 20HAZ, 40HAZ)

### Feature 8.3: Edit Mode
**Test**:
1. Click "✏️ Edit" → ✅ All cells become editable inputs
2. ✅ Currency dropdowns appear (INR/USD/EUR)
3. Enter values, select currencies

### Feature 8.4: Save / Cancel
**Test**:
1. In edit mode, change values
2. Click "💾 Save" → ✅ Toast "Saved!"
3. Reload page, select same Line+POL → ✅ Values persisted
4. Edit again, change values, click "Cancel" → ✅ Reverts to last saved

### Feature 8.5: Number Formatting
**Test**:
1. Save value "1500" in a cell
2. ✅ Displays as "1,500.00"

### Feature 8.6: Export to Excel
**Test**: Click "📥 Export" → ✅ Downloads locals_[LINE]_[POL].xlsx

---

## AREA 9: EMAIL SYSTEM (3 features)

### Feature 9.1: Email Dialog Component
**Test**:
1. Trigger email from any page (Dashboard/Entries/Completed)
2. ✅ Modal appears with "To" input field
3. ✅ Subject line shown
4. Enter comma-separated emails → ✅ All receive

### Feature 9.2: SMTP Gmail Sending
**Test**:
1. Ensure `SMTP_USER` and `SMTP_PASS` set in backend/.env
2. Send test email from any page
3. ✅ Email arrives in inbox
4. ✅ From address = SMTP_USER
5. ✅ HTML table is properly formatted

### Feature 9.3: Email Error Handling
**Test**:
1. Set wrong SMTP_PASS in .env
2. Restart backend
3. Try sending email → ✅ Toast error "Failed to send: ..."

---

## AREA 10: BACKEND API (3 features)

### Feature 10.1: Auto-create Tables
**Test**:
1. Start with empty database (just created, no tables)
2. Run `python main.py`
3. ✅ All tables created automatically
4. Check: `psql -U booking_user -d booking_portal -c "\dt"` → ✅ 10 tables listed

### Feature 10.2: Swagger API Docs
**Test**:
1. Open http://localhost:8000/docs
2. ✅ Full interactive API documentation
3. ✅ Can test any endpoint directly from browser

### Feature 10.3: Health Check
**Test**:
1. Open http://localhost:8000/api/health
2. ✅ Returns `{"status": "ok"}`

---

## QUICK SMOKE TEST (5 minutes, covers all areas)

```
1. Start backend:  cd backend && python main.py
2. Start frontend: cd frontend && npm start
3. Open http://localhost:3000
4. Click "Booking Portal"
5. Go to "Add Master" → Add Location "MUMBAI" → ✅
6. Add a Customer "TEST COMPANY" with sales person → ✅
7. Add Line "CMA CGM", POL "NHAVA SHEVA", POD "DALIAN", FPOD "DALIAN", Vessel "TEST VESSEL", Equipment "40'HC" → ✅
8. Go to "Add Booking" → Upload a PDF → ✅ Form fills
9. Submit booking → ✅ Toast success
10. Go to "Dashboard" → ✅ Pending SI = 1
11. Click Pending SI card → ✅ Entry shown
12. Click Excel → ✅ File downloads
13. Click Email → Enter your email → Send → ✅
14. Go to "View Entries" → ✅ Entry visible
15. Check SI Filed → Select BL Type → ✅
16. Check all boxes through BL Released → ✅ Moves to Completed
17. Go to "Completed Files" → ✅ Entry there
18. Go to "Manage Master" → Select Location → Edit/Delete → ✅
19. Go to "Locals" → Select Line + POL → Edit charges → Save → ✅
20. Done! All areas tested.
```
