# AEO Performance Hub

A secure, web-based Monthly Performance Reporting System for Area Education Officers (AEOs). The frontend is hosted on GitHub Pages and communicates with a Google Apps Script backend connected to a Google Sheet database.

---

## 🔗 Live App

> **[Click here to open the app](https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/)**
> *(Replace this link with your actual GitHub Pages URL after deployment)*

---

## 📌 System Overview

```
User (Browser on GitHub Pages)
        │
        │  POST request (fetch API)
        ▼
Google Apps Script Web App (API Layer)
        │
        │  Read / Write
        ▼
Google Spreadsheet (Database)
├── Sheet: Registeration   ← Staff login & profile data
├── Sheet: Open            ← KPI template for open months
└── Sheet: Close           ← Template for closed months
```

- **Frontend** → `index.html` hosted on GitHub Pages (this repository)
- **Backend** → Google Apps Script (`Code.gs`) deployed as a Web App
- **Database** → Google Sheets with sheets named `Registeration`, `Open`, and `Close`

---

## 👥 User Roles

| Role | Access |
|------|--------|
| **Admin** | Full registry management console — add/edit staff profiles, view all records, access performance dashboard for any officer |
| **User** | Performance dashboard only — fetch own profile, enter monthly KPI data, generate and download PDF report |

---

## 🔐 How Login Works

1. User opens the app and enters their **8-digit Personnel Number**
2. The app sends the number to the Google Apps Script API
3. The script checks the `Registeration` sheet (Column B) for a matching record
4. If found and **Active**, the user is logged in and routed based on their role
5. If **Inactive** or not found, an error message is shown

> There is no password. The Personnel Number is the sole credential. Keep it confidential.

---

## 📂 Repository Structure

```
your-repo/
├── index.html       ← Complete single-file frontend application
└── README.md        ← This file
```

---

## 🚀 Deployment Guide

### Step 1 — Set Up the Google Sheet

Create a Google Spreadsheet with these exact sheet names:

- `Registeration` — staff registry (Column layout below)
- `Open` — KPI template for open months
- `Close` — template for closed months

**Registeration Sheet Column Layout:**

| Col A | Col B | Col C | Col D | Col E | Col F | Col G | Col H |
|-------|-------|-------|-------|-------|-------|-------|-------|
| Sr No | Personnel No | Name | Markaz | Cell No | Status | Gmail ID | Role |

- **Status** must be either `Active` or `Inactive`
- **Role** must be either `Admin` or `User`

---

### Step 2 — Deploy the Google Apps Script

1. Open your Google Sheet → click **Extensions → Apps Script**
2. Delete any existing code and paste the full contents of `Code.gs`
3. Click **Deploy → New Deployment**
4. Set type to **Web App**
5. Set **Execute as** → `Me`
6. Set **Who has access** → `Anyone`
7. Click **Deploy** and copy the Web App URL

---

### Step 3 — Paste the API URL into index.html

Open `index.html` and find this line near the top of the `<script>` section:

```javascript
const API_URL = "https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec";
```

Replace the URL with the one you copied in Step 2.

---

### Step 4 — Enable GitHub Pages

1. Push `index.html` and `README.md` to your GitHub repository
2. Go to **Settings → Pages**
3. Under **Source**, select `main` branch and `/ (root)` folder
4. Click **Save**
5. Wait 1–2 minutes, then visit `https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/`

---

## 📋 How to Use

### For Admins

1. Log in with your 8-digit Personnel Number
2. You will land on the **Registry Console**
3. Use the **Profile Editor** (left panel) to add new officers or edit existing ones
4. Click **Modify** on any row in the table to load that officer's profile into the editor
5. Click **Performance Dashboard** (top right) to access the reporting system
6. In the dashboard, you can fetch any officer's profile and generate their report

### For Regular Users

1. Log in with your 8-digit Personnel Number
2. Your Personnel Number is pre-filled in Step 01
3. Click **Fetch** to verify your profile
4. Select the number of months to report (1–4)
5. For each month, select Month Name, Year, and School Status
   - **Open** → enter KPI data in the performance grid
   - **Closed** → no data entry needed
6. Review the live allowance estimate at the bottom of each month
7. Click **Generate & Download PDF Report** — the PDF downloads automatically

---

## 📊 KPI Grid — Data Entry Rules

| Row | Type | Rule |
|-----|------|------|
| Row 10 | Locked | Auto-filled as "Not Conducted By PMIU" |
| Rows 9, 11–17 | Numeric | Enter a value from 0 to 100 |
| Rows 18–24 | Dropdown | Select Yes or No |

- Each missed or empty target deducts **Rs 1,000** from the base allowance of **Rs 25,000**
- The live estimate updates instantly as you enter data

---

## ⚠️ Important Notes

- **Always redeploy** the Google Apps Script as a **new deployment** (not update) after making any changes to `Code.gs`, otherwise the old cached version may remain active
- The `Registeration` sheet name must be spelled exactly as shown (with the double `e`) to match the script
- The app uses `text/plain` as the Content-Type for API calls to avoid CORS errors with Google Apps Script — do not change this
- PDF generation may take up to 30 seconds depending on the number of months selected

---

## 🛠️ Troubleshooting

| Problem | Solution |
|---------|----------|
| "Personnel Number not found" | Check that the number exists in Column B of the `Registeration` sheet |
| "Profile is INACTIVE" | Change the Status field to `Active` in the registry |
| Login succeeds but page is blank | Open browser console (F12) and check for JavaScript errors |
| PDF does not download | Ensure the Google Apps Script is deployed as a Web App with access set to "Anyone" |
| Data does not save | Redeploy the Apps Script as a new deployment and update the API URL in `index.html` |
| CORS error in console | Ensure Content-Type in `apiCall()` is set to `text/plain;charset=utf-8` |

---

## 📞 Support

For access issues, contact your system administrator with your full name and Personnel Number.
