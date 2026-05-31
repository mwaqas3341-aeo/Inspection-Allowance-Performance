Here is a comprehensive and professional README.md file tailored specifically for your project. You can copy and paste this directly into your GitHub repository to document the system architecture, setup instructions, and database structure.



AEO Performance Hub 🏫📊

A fully decoupled, web-based portal designed for Assistant Education Officers (AEOs) to submit their monthly KPI reports, calculate performance allowances, and generate verifiable PDF certificates.



This project utilizes a Serverless Architecture, leveraging GitHub Pages for the frontend user interface and a Google Apps Script (GAS) Headless API for the backend database and PDF generation engine.



✨ Features

Headless API Architecture: Hosted safely on GitHub Pages, bypassing Google Account authorization restrictions by using a custom secure API.



Smart Authentication: Secure login system utilizing the officer's 8-digit Personnel Number.



Role-Based Access Control (RBAC):



Admin Console: Manage the operational core, add/edit AEO profiles, and view active network statistics.



User Dashboard: Dedicated portal for AEOs to submit monthly performance data.



Live Allowance Calculator: Real-time frontend evaluation that calculates the standard Rs 25,000 base allowance, instantly deducting Rs 1,000 for any unachieved or missing KPIs (e.g., LND, Student Attendance, SIS Data updates).



Automated PDF Engine: Generates official, strictly formatted PDF certificates in the background and triggers an instant auto-download in the user's browser, leaving zero footprint in the backend database.



Concurrency Safe: Utilizes LockService to queue simultaneous submissions, preventing server crashes or template corruption during peak end-of-month reporting.



🏗️ System Architecture

Frontend (index.html): A responsive, dark-themed UI built with Materialize CSS and standard JavaScript. Hosted via GitHub Pages. Communicates with the backend exclusively via fetch() POST requests.



Backend (Code.gs): A Google Apps Script deployed as a Web App API. It intercepts POST requests, validates users, evaluates target percentages, securely generates temporary throwaway spreadsheets for PDF conversion, and returns Base64 data to the frontend.



Database (Google Sheets): Acts as the master database. Contains registration data, exact KPI targets, entitlements, and the official PDF certificate templates.



🛠️ Setup \& Deployment Guide

Phase 1: Backend (Google Sheets \& Apps Script)

Create a new Google Spreadsheet.



Create the following three specific tabs exactly as named:



Registeration: The master staff directory.



Open: The template used for active school months.



Close: The template used for closed/vacation months.



Click Extensions > Apps Script.



Delete any default code, paste the contents of Code.gs, and click Save.



Click Deploy > New deployment.



Configure the deployment exactly as follows:



Select type: Web app



Execute as: Me (Crucial for granting the script permission to generate PDFs)



Who has access: Anyone (Crucial for allowing GitHub Pages to communicate with it)



Click Deploy, authorize the permissions, and Copy the Web App URL.



Phase 2: Frontend (GitHub Pages)

Create a new repository on GitHub.



Create an index.html file and paste the frontend HTML/JS code.



Locate the API\_URL variable at the very top of the <script> tag:



JavaScript

const API\_URL = "PASTE\_YOUR\_GOOGLE\_WEB\_APP\_URL\_HERE"; 

4\. Replace the placeholder with the URL you copied in Phase 1.

5\. Commit the changes.

6\. Go to your repository \*\*Settings > Pages\*\*.

7\. Set the source to the `main` branch and click \*\*Save\*\*. Your site will be live within a few minutes.



\---



\## 🗄️ Database Structure



For the API to function correctly, your Google Sheet must be formatted with the following columns and logic:



\### The `Registeration` Sheet

Data must begin on Row 2 (Row 1 is assumed to be headers).

\* \*\*Column B (2):\*\* Personnel Number (8 digits)

\* \*\*Column C (3):\*\* Name

\* \*\*Column D (4):\*\* Markaz

\* \*\*Column E (5):\*\* Cell Number

\* \*\*Column F (6):\*\* Status (`Active` or `Inactive`)

\* \*\*Column G (7):\*\* Gmail ID

\* \*\*Column H (8):\*\* Role (`User` or `Admin`)



\### The `Open` Template Sheet

\* \*\*A9:A24:\*\* Serial Numbers

\* \*\*B9:B24:\*\* Indicator Labels (e.g., Student Retention, Teacher Presence)

\* \*\*C9:C24:\*\* Target % or required standards

\* \*\*E9:E24:\*\* Entitlements

\* \*\*B27:\*\* The dedicated cell where the final calculated financial allowance (Base 25,000 - deductions) will be injected by the backend before PDF conversion.



\---



\## 🔒 Security Notes

\* The Google Spreadsheet serves strictly as a \*\*Read-Only\*\* template during generation. The backend creates an invisible, temporary "throwaway" file to process the PDF and instantly trashes it, ensuring your master templates are never overwritten by simultaneous user traffic.

\* Admin privileges are strictly dictated by Column H in the database, ensuring frontend manipulations cannot grant elevated privileges.

