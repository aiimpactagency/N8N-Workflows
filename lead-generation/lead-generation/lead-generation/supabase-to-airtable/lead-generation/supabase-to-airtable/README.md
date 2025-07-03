# ⚡ Supabase to Airtable – Lead Capture Workflow

This automation captures new leads added to a Supabase table, validates the email address, logs errors for invalid emails, stores valid leads in Airtable, and sends a welcome email using Gmail. It also updates the Supabase record with the result (success or error).

---

## ✅ What’s Included

✅ Supabase trigger on new lead row  
✅ Lead data extraction and formatting  
✅ Email format validation  
✅ Leads saved to Airtable  
✅ Welcome email sent via Gmail  
✅ Error handling for invalid emails  
✅ Logs errors in Airtable  
✅ Updates Supabase row with status (success/error)

---

## 📄 Documentation

📘 [Overview – What This Workflow Does]
🛠️ [Setup Instructions – How to Use It]

---

## 📁 Files in This Folder

| File                         | Description                                  |
|------------------------------|----------------------------------------------|
| `supabase-to-airtable.json`  | n8n workflow (import this into your account) |
| `overview.md`                | What the workflow automates, use cases       |
| `setup-instructions.md`      | Step-by-step install and credential setup    |
| `README.md`                  | This summary and quick access to docs        |

---

## 🚀 How to Use

1. Click the `.json` file and download the raw contents  
2. Save as `supabase-to-airtable.json`  
3. Import into [n8n.io](https://n8n.io) via **Workflows > Import from File**  
4. Configure:
   - Supabase credentials (Database trigger)  
   - Airtable API key and base ID  
   - Gmail OAuth2 credentials  
5. Follow the [Setup Instructions](./setup-instructions.md)  

---

💬 DM me on TikTok or leave an issue if you get stuck or want to request more workflows!
