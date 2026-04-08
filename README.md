# ⚓ DMS Booking Portal

Freight forwarding booking management system with AI-powered PDF extraction, 100+ analytics KPIs, and automated email notifications.

## Architecture
- **Backend**: Python FastAPI + PostgreSQL + SQLAlchemy
- **Frontend**: React 18 + MUI DataGrid + Recharts
- **AI**: Gemini Gemini 2.0 Flash (optional, for PDF extraction + analytics)
- **Email**: SMTP Gmail

## Quick Start
```bash
# 1. Database
sudo -u postgres psql -c "CREATE USER booking_user WITH PASSWORD 'booking_pass';"
sudo -u postgres psql -c "CREATE DATABASE booking_portal OWNER booking_user;"

# 2. Backend
cd backend
cp .env.example .env          # Edit with your credentials
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
python main.py

# 3. Frontend (new terminal)
cd frontend
npm install
npm start
```

## Files (30 files, ~5000 lines)
```
booking-portal/
├── backend/
│   ├── .env.example     ← Copy to .env, add your keys here
│   ├── database.py      ← DB connection + .env loader
│   ├── models.py        ← 13 tables (Entry, SalesPerson, AuditLog, etc.)
│   ├── main.py          ← All API endpoints (~1000 lines)
│   └── requirements.txt
├── frontend/
│   ├── .env.example     ← API URL config
│   ├── src/
│   │   ├── pages/       ← 9 pages (Dashboard, Analytics, etc.)
│   │   ├── components/  ← Sidebar, EmailDialog
│   │   └── utils/       ← Email builder
│   └── package.json
├── SETUP.md             ← Full setup guide
├── FEATURES.md          ← 152 feature roadmap
├── TESTING.md           ← 67 existing feature tests
└── TESTING_NEW_FEATURES.md ← 36 new feature tests
```

## Key Features
- 📄 PDF import (20+ carriers, Gemini AI or regex)
- 📊 Dashboard with 7 metric cards
- 📈 Analytics with 30+ KPIs, charts, tables
- 📧 Auto-emails (booking confirm, BL release, SI reminders)
- 💰 Dynamic locals charges grid
- 👤 Separate sales person management
- 🤖 AI natural language query, smart summary, customer reports
- 📥 Excel export on every table
- 🔍 Duplicate detection, validity alerts, aging analysis
