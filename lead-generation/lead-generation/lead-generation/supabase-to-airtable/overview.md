# 📘 Overview – What This Workflow Does

The **Supabase to Airtable – Lead Capture Workflow** is an n8n automation that simplifies how new leads are collected, validated, and processed.

Whenever a new row is added to your **Supabase lead table**, this workflow:

1. **Extracts & formats** the data  
2. **Validates the email address** format  
3. If valid:
   - Creates a new lead in **Airtable**
   - Sends a **welcome email** using Gmail
   - Updates the Supabase record with a success status  
4. If invalid:
   - Logs the error in a dedicated Airtable table
   - Sends an error response
   - Updates the Supabase record with an error status

---

## 🎯 Use Cases

✅ Automate lead management from Supabase forms or landing pages  
✅ Remove manual validation and follow-up tasks  
✅ Keep Airtable CRM updated in real-time  
✅ Immediately notify valid leads with a welcome email  
✅ Flag invalid or missing emails for review  

---

## 💼 Who It's For

- **Startup founders** capturing early leads via custom forms  
- **No-code builders** using Supabase as a backend  
- **Marketing teams** wanting to sync leads to Airtable  
- **Developers** needing a scalable, low-latency trigger-based workflow  
- Anyone wanting **clean, validated lead data** sent to Airtable automatically

---

## 🧱 Tech Stack

- **Supabase** (row-level event trigger)
- **Airtable** (lead storage and error logging)
- **Gmail** (automated email delivery)
- **n8n** (workflow orchestration)

---

## ⚡ Workflow Flowchart

