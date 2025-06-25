https://docs.google.com/document/d/1pRYGDBshHKH3Pu2UaM9t9rtzlu7OQ-Mf-IWFUNF_bKo/edit?tab=t.0

Webhook Lead Processing Workflow - Setup & Credential Instructions
Prerequisites
Before implementing this workflow, ensure you have:
N8N Instance: Self-hosted or cloud version (v1.0+)
Airtable Account: With API access enabled
Gmail Account: With OAuth2 enabled or App Password
Typeform Account (Optional): If using Typeform as source
Domain/Server: For webhook endpoint (if self-hosted)
Credential Setup
1. Airtable API Credentials
Steps to Configure:
Get Airtable API Key:


Log into Airtable
Go to Account Settings
Navigate to "API" section
Click "Generate API key" if you don't have one
Copy your API key
Get Base and Table IDs:


Open your Airtable base
Go to "Help" → "API documentation"
Find your Base ID (starts with app)
Find your Table IDs (starts with tbl)
Configure in N8N:


Go to Credentials → Add Credential → Airtable
Name: "Airtable API"
API Key: Paste your API key
Save
Update Workflow:


Replace appXXXXXXXXXXXXXX with your Base ID
Replace tblXXXXXXXXXXXXXX with your Leads table ID
Replace tblYYYYYYYYYYYYYY with your Failed_Leads table ID
Required Airtable Tables:
Leads Table Schema:
- Name (Single line text)
- Email (Email)
- Phone (Phone number)
- Company (Single line text)
- Message (Long text)
- Source (Single select: Typeform, Webhook, Manual)
- Status (Single select: New, Contacted, Qualified, Closed)
- Created (Date)
- Lead Score (Number)
- UTM Source (Single line text)
- UTM Medium (Single line text)
- UTM Campaign (Single line text)
- IP Address (Single line text)

Failed_Leads Table Schema:
- Error Type (Single line text)
- Raw Data (Long text)
- Timestamp (Date)
- Source (Single line text)
- Attempted Email (Single line text)
- Full Name (Single line text)
- Error Message (Long text)

2. Gmail OAuth2 Credentials
Method A: OAuth2 (Recommended)
Enable Gmail API:


Go to Google Cloud Console
Create new project or select existing
Enable Gmail API
Create OAuth2 credentials
Configure OAuth2:


Application type: Web application
Authorized redirect URI: https://your-n8n-url.com/rest/oauth2-credential/callback
Copy Client ID and Client Secret
Setup in N8N:


Add Credential → Gmail OAuth2
Client ID: Your OAuth2 client ID
Client Secret: Your OAuth2 client secret
Click "Connect" and authorize
Method B: App Password (Alternative)
Enable 2FA on your Google account
Generate App Password:
Go to Google Account Security
Select "App passwords"
Generate password for "Mail"
Use in N8N with SMTP credentials
3. Webhook Configuration
For N8N Cloud:
Your webhook URL will be: https://your-workspace.n8n.cloud/webhook/lead-capture
No additional setup required
For Self-Hosted N8N:
Configure Webhook URL:

 N8N_WEBHOOK_URL=https://your-domain.com


SSL Certificate (Required for production):


Use Let's Encrypt or commercial certificate
Configure in reverse proxy (nginx/Apache)
Firewall Rules:


Open port 443 (HTTPS)
Whitelist Typeform IPs if using IP filtering
Environment Variables
Add these to your N8N environment:
# Core Settings
N8N_WEBHOOK_URL=https://your-domain.com
N8N_PROTOCOL=https
N8N_HOST=your-domain.com
N8N_PORT=5678

# Email Settings (if using SMTP)
N8N_EMAIL_MODE=smtp
N8N_SMTP_HOST=smtp.gmail.com
N8N_SMTP_PORT=465
N8N_SMTP_SSL=true

# Optional Security
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=your-password

Permission Scopes
Airtable Permissions:
Read: View records in base
Write: Create records
Schema Read: View base schema
Gmail Permissions:
gmail.send: Send emails on your behalf
gmail.compose: Compose email drafts
Webhook Permissions:
No special permissions required
Optional: IP whitelisting for security
Typeform Integration (Optional)
Create Typeform:


Design your lead capture form
Add required fields: name, email, phone, company, message
Configure Webhook:


Go to Connect → Webhooks
Add webhook URL: https://your-n8n-url.com/webhook/lead-capture
Select events: "form_response"
Add Hidden Fields (Optional):


lead_id
utm_source
utm_medium
utm_campaign
Testing & Validation
1. Test Webhook Endpoint:
curl -X POST https://your-n8n-url.com/webhook/lead-capture \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test User",
    "email": "test@example.com",
    "phone": "+1234567890",
    "company": "Test Company",
    "message": "This is a test submission"
  }'

2. Verify Airtable Connection:
Check if test record appears in Leads table
Verify all fields are mapped correctly
3. Confirm Email Delivery:
Check if welcome email is received
Verify personalization tokens work
Test with different email providers
Troubleshooting
Common Setup Errors:
Webhook Not Responding:


Check N8N_WEBHOOK_URL environment variable
Verify SSL certificate is valid
Ensure port 443 is open
Airtable Connection Failed:


Verify API key is correct
Check Base ID and Table IDs
Ensure tables have correct schema
Gmail Send Failure:


Re-authenticate OAuth2 connection
Check if less secure app access is enabled
Verify sender email matches authenticated account
Email Validation Failing Valid Emails:


Review regex pattern in IF node
Test with various email formats
Consider using email validation service
Debug Mode:
Enable verbose logging in N8N:
N8N_LOG_LEVEL=debug
N8N_LOG_OUTPUT=console

Security Best Practices
Webhook Security:


Use HTTPS only
Implement webhook signature validation
Add rate limiting
Use webhook tokens
Data Protection:


Encrypt sensitive data in Airtable
Use environment variables for credentials
Implement GDPR compliance measures
Regular backup of Airtable data





