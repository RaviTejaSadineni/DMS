# SETUP.md — Complete Setup Guide

---

## Step 1: Install Software

### Python 3.10+
- **Windows**: https://www.python.org/downloads/ → Download 3.12 → ✅ Check **"Add to PATH"** → Install
- **Mac**: `brew install python`
- **Linux**: `sudo apt update && sudo apt install python3 python3-pip python3-venv`
- **Verify**: `python --version` (or `python3 --version`)

### Node.js 18+
- **Windows/Mac**: https://nodejs.org → Download **LTS** → Install
- **Linux**: `curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - && sudo apt install -y nodejs`
- **Verify**: `node --version` and `npm --version`

### PostgreSQL 16
- **Windows**: https://www.postgresql.org/download/windows/ → EDB installer → Set password for `postgres` user → Keep port `5432`
- **Mac**: `brew install postgresql@16 && brew services start postgresql@16`
- **Linux**: `sudo apt install postgresql postgresql-contrib && sudo systemctl start postgresql && sudo systemctl enable postgresql`

---

## Step 2: Create Database

Connect to PostgreSQL:

**Windows** — Open "SQL Shell (psql)" from Start Menu:
```
Server: localhost
Database: postgres
Port: 5432
Username: postgres
Password: (whatever you set during installation)
```

**Mac/Linux**:
```bash
sudo -u postgres psql
```

Run these 3 commands:
```sql
CREATE USER booking_user WITH PASSWORD 'booking_pass';
CREATE DATABASE booking_portal OWNER booking_user;
GRANT ALL PRIVILEGES ON DATABASE booking_portal TO booking_user;
```

Type `\q` to exit.

**Verify it worked:**
```bash
psql -U booking_user -d booking_portal -h localhost
# Enter password: booking_pass
# If you see "booking_portal=>" prompt, it's working. Type \q to exit.
```

---

## Step 3: Create Backend .env File

This is where ALL your secrets and configuration go.

```bash
cd booking-portal/backend
```

**Windows (Command Prompt):**
```bash
copy .env.example .env
```

**Mac/Linux:**
```bash
cp .env.example .env
```

Now open `backend/.env` in any text editor (Notepad, VS Code, nano) and fill it in:

```bash
# =============================================
# BACKEND CONFIGURATION — backend/.env
# =============================================

# ---------- DATABASE (REQUIRED) ----------
# Format: postgresql://USERNAME:PASSWORD@HOST:PORT/DATABASE_NAME
# If you followed Step 2 exactly, keep this as-is:
DATABASE_URL=postgresql://booking_user:booking_pass@localhost:5432/booking_portal

# ---------- EMAIL / SMTP (REQUIRED for email features) ----------
# Used by: Dashboard "📧 Email", View Entries "📧 Email", Completed Files "📧 Email"
#
# To use Gmail:
#   1. Go to https://myaccount.google.com/security
#   2. Enable 2-Step Verification (required)
#   3. Go to https://myaccount.google.com/apppasswords
#   4. Select "Mail" and "Other (Custom name)" → name it "Booking Portal"
#   5. Copy the 16-character password (like: abcd efgh ijkl mnop)
#   6. Paste below WITHOUT spaces
#
SMTP_USER=tanks@dessertmarine.com
SMTP_PASS=awoukyetgbgsbtud
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587

# ---------- AZURE OPENAI (OPTIONAL — for AI PDF extraction) ----------
# Used by: Add Booking → Import from PDF (Feature #1 in FEATURES.md)
# Currently the parser uses regex. When you add these keys and implement
# Feature #1, it will use Azure OpenAI GPT-4o vision for intelligent extraction.
#
# To get these values:
#   1. Go to https://portal.azure.com
#   2. Search "Azure OpenAI" → Create a resource (or use existing)
#   3. Go to your resource → "Keys and Endpoint" in left menu
#   4. Copy "Endpoint" → paste as AZURE_OPENAI_ENDPOINT
#   5. Copy "Key 1" → paste as AZURE_OPENAI_KEY
#   6. Go to "Model deployments" → "Manage Deployments"
#   7. Deploy a model (recommended: gpt-4o)
#   8. The deployment name you chose → paste as AZURE_OPENAI_DEPLOYMENT
#
# Leave blank if not using AI extraction (regex parser will be used instead):
AZURE_OPENAI_ENDPOINT=
AZURE_OPENAI_KEY=
AZURE_OPENAI_DEPLOYMENT=gpt-4o
AZURE_OPENAI_API_VERSION=2024-02-15-preview

# ---------- AZURE BLOB STORAGE (OPTIONAL — for storing uploaded PDFs) ----------
# Used by: Feature #12 in FEATURES.md (PDF attachment storage)
# Not yet implemented in current code — for future use.
#
# To get these values:
#   1. Go to https://portal.azure.com
#   2. Create a Storage Account (or use existing)
#   3. Go to "Access keys" in left menu
#   4. Copy "Connection string" → paste below
#   5. Create a container named "booking-pdfs" in Blob service
#
# Leave blank if not using:
AZURE_STORAGE_CONNECTION_STRING=
AZURE_STORAGE_CONTAINER=booking-pdfs
```

### How .env loading works

```
backend/.env file
    ↓
database.py loads it on startup using python-dotenv:
    load_dotenv(Path(__file__).parent / ".env")
    ↓
All values available via os.getenv("KEY_NAME") anywhere in backend code:
    - database.py reads DATABASE_URL
    - main.py reads SMTP_USER, SMTP_PASS, SMTP_HOST, SMTP_PORT
    - main.py reads AZURE_OPENAI_* (when you implement Feature #1)
```

### Where each key is used in the code

| Key | File | Line | Used For |
|-----|------|------|----------|
| `DATABASE_URL` | `backend/database.py` | line 11 | PostgreSQL connection |
| `SMTP_USER` | `backend/main.py` | line 922 | Email "From" address |
| `SMTP_PASS` | `backend/main.py` | line 923 | Gmail App Password |
| `SMTP_HOST` | `backend/main.py` | line 924 | SMTP server hostname |
| `SMTP_PORT` | `backend/main.py` | line 925 | SMTP server port |
| `AZURE_OPENAI_ENDPOINT` | `backend/main.py` | (future) | Azure OpenAI API URL |
| `AZURE_OPENAI_KEY` | `backend/main.py` | (future) | Azure OpenAI API key |
| `AZURE_OPENAI_DEPLOYMENT` | `backend/main.py` | (future) | Model deployment name |
| `AZURE_OPENAI_API_VERSION` | `backend/main.py` | (future) | API version string |

---

## Step 4: Create Frontend .env File (Optional)

Only needed if your backend runs on a different URL (not localhost:8000).

```bash
cd booking-portal/frontend
```

**Windows:** `copy .env.example .env`
**Mac/Linux:** `cp .env.example .env`

Open `frontend/.env`:

```bash
# =============================================
# FRONTEND CONFIGURATION — frontend/.env
# =============================================

# Backend API URL
# Default: http://localhost:8000/api (works for local development)
# Change this when deploying to production server:
REACT_APP_API_URL=http://localhost:8000/api
```

### How frontend .env works

```
frontend/.env
    ↓
React (create-react-app) auto-reads all REACT_APP_* variables
    ↓
frontend/src/api.js reads it:
    baseURL: process.env.REACT_APP_API_URL || "http://localhost:8000/api"
```

**Important**: After changing `frontend/.env`, you must restart the frontend (`npm start`).

---

## Step 5: Start Backend

Open **Terminal 1**:

```bash
cd booking-portal/backend
```

Create virtual environment (first time only):
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Mac/Linux
python3 -m venv venv
source venv/bin/activate
```

Install dependencies (first time only):
```bash
pip install -r requirements.txt
```

Start:
```bash
python main.py
```

Expected output:
```
INFO:     Uvicorn running on http://0.0.0.0:8000
```

**Verify:**
- Open http://localhost:8000/api/health → should show `{"status":"ok"}`
- Open http://localhost:8000/docs → should show Swagger API docs

**Note:** Database tables are auto-created on first startup. No manual SQL needed.

---

## Step 6: Start Frontend

Open **Terminal 2** (keep Terminal 1 running):

```bash
cd booking-portal/frontend
npm install       # first time only
npm start
```

Expected: Browser opens http://localhost:3000 automatically.

---

## Step 7: Quick Verification

1. Click **Booking Portal** in sidebar
2. Go to **Add Master** → expand Location → add "MUMBAI" → click ✅ Add
3. Go to **Dashboard** → verify "MUMBAI" button appears
4. Go to **Add Booking** → upload a carrier PDF → verify form fills
5. Submit a booking → go to **View Entries** → verify it appears
6. Click **📧 Email** on View Entries → enter your email → send → check inbox

If all 6 steps pass, everything is working.

---

## File Reference

```
booking-portal/
├── .gitignore               ← Excludes .env files from git
├── README.md                ← Quick reference
├── SETUP.md                 ← This file
├── FEATURES.md              ← 152 future feature ideas
├── TESTING.md               ← 67 test cases for all features
│
├── backend/
│   ├── .env.example         ← TEMPLATE — copy to .env and fill in
│   ├── .env                 ← YOUR CONFIG (create from .env.example)
│   ├── database.py          ← Loads .env, connects to PostgreSQL
│   ├── models.py            ← Database table definitions
│   ├── main.py              ← All API endpoints
│   └── requirements.txt     ← Python dependencies
│
└── frontend/
    ├── .env.example          ← TEMPLATE — copy to .env (optional)
    ├── .env                  ← YOUR CONFIG (optional, for API URL)
    ├── package.json          ← React dependencies
    ├── public/index.html
    └── src/
        ├── api.js            ← Reads REACT_APP_API_URL from .env
        ├── index.js
        ├── App.js
        ├── components/       ← Sidebar.js, EmailDialog.js
        ├── utils/            ← emailUtils.js
        └── pages/            ← All 9 page components
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `python: command not found` | Use `python3` instead, or reinstall with "Add to PATH" |
| `No module named 'dotenv'` | Run `pip install python-dotenv` |
| `psycopg2` install fails | Run `pip install psycopg2-binary` |
| `connection refused` to database | Start PostgreSQL: `sudo systemctl start postgresql` (Linux) or check Windows Services |
| Backend starts but tables not created | Check `DATABASE_URL` in `.env` — must match your database credentials |
| Email fails "authentication error" | You need a Gmail **App Password**, not your regular Gmail password. Enable 2FA first. |
| Email fails "connection refused" | Check `SMTP_HOST` and `SMTP_PORT` in `.env` |
| Frontend shows "Network Error" | Backend not running, or `REACT_APP_API_URL` is wrong in `frontend/.env` |
| PDF import returns empty fields | PDF must have extractable text (not scanned). Test with http://localhost:8000/docs → POST /api/parse-pdf |
| Changes to `.env` not taking effect (backend) | Restart backend: stop and run `python main.py` again |
| Changes to `.env` not taking effect (frontend) | Restart frontend: stop and run `npm start` again |
| Port 8000 in use | Change in `main.py` last line: `uvicorn.run(app, port=8001)` and update `frontend/.env` |
| Port 3000 in use | React prompts "use another port?" — type Y |
