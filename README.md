# DMS Booking Portal

A full-stack freight forwarding booking management system.

- **Frontend**: React 18 + MUI DataGrid + Recharts
- **Backend**: Python FastAPI + PostgreSQL 16
- **PDF Import**: Automatic field extraction from carrier booking PDFs (Arkas, CMA CGM, Diamond Maritime, etc.)

---

## Prerequisites — Install These First

### 1. Python 3.10+ (for Backend)

**Windows:**
1. Go to https://www.python.org/downloads/
2. Download Python 3.12 (or latest 3.x)
3. **IMPORTANT**: During installation, check ✅ **"Add Python to PATH"**
4. Click "Install Now"
5. Verify: Open Command Prompt → type `python --version`

**Mac:**
```bash
brew install python
```

**Linux (Ubuntu):**
```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv
```

---

### 2. Node.js 18+ (for Frontend)

**Windows / Mac:**
1. Go to https://nodejs.org/
2. Download the **LTS** version (18.x or 20.x)
3. Run the installer (accept defaults)
4. Verify: Open Command Prompt → type `node --version` and `npm --version`

**Linux (Ubuntu):**
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```

---

### 3. PostgreSQL 16

**Windows:**
1. Go to https://www.postgresql.org/download/windows/
2. Download the installer from EDB (EnterpriseDB)
3. Run installer — accept defaults
4. Set a password for the `postgres` superuser (remember this!)
5. Keep the default port `5432`
6. After installation, open **pgAdmin 4** (installed with PostgreSQL) or use command line

**Mac:**
```bash
brew install postgresql@16
brew services start postgresql@16
```

**Linux (Ubuntu):**
```bash
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

---

### 4. Create the Database and User

Open a terminal/command prompt and connect to PostgreSQL:

**Windows** (use SQL Shell (psql) from Start Menu):
```
Server: localhost
Database: postgres
Port: 5432
Username: postgres
Password: (your password from installation)
```

**Mac/Linux:**
```bash
sudo -u postgres psql
```

Then run these SQL commands:
```sql
CREATE USER booking_user WITH PASSWORD 'booking_pass';
CREATE DATABASE booking_portal OWNER booking_user;
GRANT ALL PRIVILEGES ON DATABASE booking_portal TO booking_user;
\q
```

---

## Project Setup

### Step 1: Get the Project Files

Place the project folder anywhere on your computer. You should have:
```
booking-portal/
├── backend/
│   ├── main.py
│   ├── models.py
│   ├── database.py
│   └── requirements.txt
├── frontend/
│   ├── public/
│   │   └── index.html
│   ├── src/
│   │   ├── api.js
│   │   ├── App.js
│   │   ├── index.js
│   │   ├── components/
│   │   │   └── Sidebar.js
│   │   └── pages/
│   │       ├── Home.js
│   │       ├── BookingPortal.js
│   │       ├── Dashboard.js
│   │       ├── AddBooking.js
│   │       ├── ViewEntries.js
│   │       ├── CompletedFiles.js
│   │       ├── AddMaster.js
│   │       ├── ManageMaster.js
│   │       └── Locals.js
│   └── package.json
└── README.md
```

---

### Step 2: Start the Backend

Open **Terminal 1** (Command Prompt / PowerShell / Terminal):

```bash
cd booking-portal/backend
```

Create a virtual environment:

**Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

**Mac/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

Install dependencies:
```bash
pip install -r requirements.txt
```

(Optional) If your PostgreSQL credentials are different, set the DATABASE_URL:

**Windows:**
```bash
set DATABASE_URL=postgresql://booking_user:booking_pass@localhost:5432/booking_portal
```

**Mac/Linux:**
```bash
export DATABASE_URL=postgresql://booking_user:booking_pass@localhost:5432/booking_portal
```

Start the backend server:
```bash
python main.py
```

You should see:
```
INFO:     Uvicorn running on http://0.0.0.0:8000
```

The API is now running at **http://localhost:8000**
- API docs: http://localhost:8000/docs (Swagger UI)
- Health check: http://localhost:8000/api/health

---

### Step 3: Start the Frontend

Open **Terminal 2** (keep Terminal 1 running):

```bash
cd booking-portal/frontend
```

Install dependencies:
```bash
npm install
```

Start the development server:
```bash
npm start
```

This will open **http://localhost:3000** in your browser.

---

## Using the Application

### Navigation
- **Sidebar** has two tabs: **Home** and **Booking Portal**
- Click **Booking Portal** to see the full tabbed interface with all 7 pages

### Pages

| Tab | Description |
|-----|-------------|
| **Dashboard** | Metrics cards (Pending SI, First Print, Correction, BL, Invoice, DG, Empty Pickup), bar chart by ETD, location filter (MUMBAI/GUJARAT from backend), date range filter. Click any card to see filtered entries. |
| **Add Booking** | Full booking form with all fields. **Import from PDF** button parses carrier booking PDFs and auto-fills fields. Supports add-new for all dropdown fields (Location, Customer, Line, POL, POD, FPOD, Vessel, Equipment Type). |
| **View Entries** | MUI DataGrid with inline editing, checkbox toggles (VGM, SI, DG, First Print, Corrections, Invoice, BL Released), location filter, search, Excel export, duplicate, delete. BL Released moves entry to Completed. |
| **Completed Files** | Read-only DataGrid of completed bookings (BL Released = true), with location filter and search. |
| **Add Master** | Accordion cards to add new master data for all 7 categories (Customer, Line, POL, POD, FPOD, Vessel, Equipment Type) with all fields. |
| **Manage Master** | Full table view with search, inline edit, save/cancel, delete for all master data categories. |
| **Locals** | Charge grid per Line + POL combination. Editable grid with currency selectors (INR/USD/EUR). Save, cancel, export to Excel. |

### PDF Import
- In **Add Booking**, click the file input next to "Import from PDF"
- Upload a carrier booking confirmation PDF (Arkas, CMA CGM, Diamond Maritime, or similar)
- The system extracts: Booking No, Date, Validity, Customer, Line, Vessel/Voyage, POL, POD, FPOD, ETD, Equipment details, Container No, etc.
- Handles abbreviated fields (e.g., "Qty &Type of Containers", "Vessel/Voyage", date formats like "30-MAR-26" or "08-APR-2026 02:30")
- Extracted data auto-fills the form; you can review and edit before submitting
- The PDF name appears as an attachment badge that can be replaced

---

## Database Configuration

Default connection string:
```
postgresql://booking_user:booking_pass@localhost:5432/booking_portal
```

To change, set the `DATABASE_URL` environment variable before starting the backend.

Tables are auto-created on first startup via SQLAlchemy `create_all()`.

---

## API Endpoints Reference

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/health` | Health check |
| GET/POST | `/api/locations` | List / Create locations |
| GET/POST | `/api/customers` | List / Create customers |
| PUT/DELETE | `/api/customers/{id}` | Update / Delete customer |
| GET/POST | `/api/lines` | List / Create lines |
| GET/POST | `/api/pols` | List / Create POLs |
| GET/POST | `/api/pods` | List / Create PODs |
| GET/POST | `/api/fpods` | List / Create FPODs |
| GET/POST | `/api/vessels` | List / Create vessels |
| GET/POST | `/api/equipment-types` | List / Create equipment types |
| GET | `/api/entries?status=active` | List active entries |
| GET | `/api/entries?status=completed` | List completed entries |
| POST | `/api/entries` | Create new entry |
| PUT | `/api/entries/{id}` | Update entry |
| DELETE | `/api/entries/{id}` | Delete entry |
| POST | `/api/entries/{id}/duplicate` | Duplicate entry |
| GET | `/api/dashboard` | Dashboard metrics |
| GET/POST | `/api/locals` | Get / Save local charges |
| POST | `/api/parse-pdf` | Parse booking PDF |

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `python` not found | Use `python3` instead, or reinstall Python with "Add to PATH" checked |
| PostgreSQL connection refused | Make sure PostgreSQL service is running: `sudo systemctl start postgresql` (Linux) or check Windows Services |
| `psycopg2` install fails | Try `pip install psycopg2-binary` or install PostgreSQL dev headers |
| Port 8000 in use | Change port in `main.py`: `uvicorn.run(app, port=8001)` |
| Port 3000 in use | React will ask to use another port — type `Y` |
| CORS errors | Backend already allows all origins. Make sure backend is running on port 8000. |
| PDF parsing returns empty | Check that the PDF has extractable text (not scanned images). pdfplumber works with text-based PDFs. |
