# 🛠 Setup Instructions – How to Use This Workflow

This guide walks you through setting up the **Supabase to Airtable – Lead Capture Workflow** in n8n. You'll connect your Supabase table, Airtable base, and Gmail account to make the automation work end-to-end.

---

## ✅ Step 1: Import the Workflow into n8n

1. Download the file `supabase-to-airtable.json` from this repo  
2. Open [n8n.io](https://n8n.io) (cloud or self-hosted)  
3. Go to **Workflows > Import from File**  
4. Select the downloaded file  
5. Click **“Execute Workflow”** to test manually once everything is configured  

---

## 🧩 Step 2: Configure Supabase Trigger

This workflow listens for new rows in your Supabase table.

1. In Supabase:
   - Go to your **lead table**
   - Enable **Row-level Security (RLS)** if needed
   - Set up a webhook trigger on “INSERT” (row created)

2. In n8n:
   - Open the `Supabase Lead Trigger` node
   - Add:
     - **Project URL** (e.g., `https://xyzcompany.supabase.co`)
     - **API Key** (from your Supabase project settings)
     - **Table Name** (e.g., `leads`)
     - **Schema**: usually `public`

✅ Make sure this node is set to **"Trigger on: Create Row"**

---

## 📋 Step 3: Configure Airtable Nodes

You’ll need two Airtable nodes:
- One for saving valid leads
- One for logging invalid emails

1. Open both **Airtable** nodes in the workflow:
   - Paste your **API Key**
   - Set the **Base ID** (you can find this in your Airtable URL)
   - Select or type the **Table Name** (e.g., `Valid Leads`, `Lead Errors`)
   - Map the fields from Supabase into Airtable columns (e.g., Name, Email, Source)

---

## 📧 Step 4: Set Up Gmail (for Welcome Email)

1. Open the `Send Welcome Email` node  
2. Use **OAuth2** to connect your Gmail account securely  
3. Customize:
   - **From/To** email fields (usually dynamic from the Supabase row)
   - **Subject** (e.g., "Thanks for Signing Up!")
   - **Message body** – personalize using variables from input (like name or topic)

---

## 🔁 Step 5: Supabase Update Nodes

There are two **Supabase: Update Row** nodes:
- `Update Supabase – Success`
- `Update Supabase – Error`

1. These mark the original row with status fields (e.g., `status = success` or `error`)
2. Make sure the **Row ID** from the original trigger is being passed into these nodes correctly
3. Confirm field names match your Supabase schema

---

## 🧪 Step 6: Test the Workflow

1. In Supabase, manually add a new row to your `leads` table  
2. Watch the workflow run in real time in n8n  
3. Check:
   - Airtable: did the lead or error log appear?
   - Gmail: did the welcome email send?
   - Supabase: did the row update with `status = success`?

---

## 🛟 Troubleshooting Tips

- ❌ Email not sending? → Check Gmail OAuth connection and rate limits  
- ❌ Record not saving in Airtable? → Double-check table names and field mappings  
- ❌ Supabase not triggering? → Ensure webhook is set up and API key is correct  
- ❌ Nothing happening? → Add a `Debug` node between steps to inspect input/output

---

You’re now fully set up. Just connect credentials once and this workflow runs automatically for every new lead 🎉
