# Social Media Content Generation Workflow
## Setup & Credential Instructions

### Prerequisites

Before setting up this workflow, ensure you have access to the following accounts and services:

#### Required Accounts
1. **N8N Instance** (Self-hosted or N8N Cloud)
2. **SerpAPI Account** (for search result scraping)
3. **OpenAI Account** (for GPT-4 access)
4. **Gmail Account** (for email delivery)

#### Required Subscriptions
- **SerpAPI**: Free plan (100 searches/month) or paid plan for higher volume
- **OpenAI**: Pay-per-use API access with GPT-4 availability
- **Gmail**: Free Gmail account with app-specific password capability

### Step-by-Step Setup

#### 1. SerpAPI Configuration

**Account Setup:**
1. Visit [SerpAPI.com](https://serpapi.com)
2. Create a free account or sign in
3. Navigate to "Dashboard" → "API Key"
4. Copy your API key (format: `your_api_key_here`)

**N8N Credential Setup:**
1. In N8N, go to "Credentials" → "Create New"
2. Search for "HTTP Header Auth" 
3. Configure as follows:
   - **Credential Name**: `SerpAPI Key`
   - **Header Name**: `X-API-KEY`
   - **Header Value**: `[Your SerpAPI Key]`
4. Save the credential

**API Limits & Billing:**
- **Free Plan**: 100 searches/month
- **Paid Plans**: Start at $50/month for 5,000 searches
- **Rate Limiting**: 1 request per second on free plan
- **Recommended**: Start with free plan for testing

#### 2. OpenAI API Configuration

**Account Setup:**
1. Visit [platform.openai.com](https://platform.openai.com)
2. Create account or sign in
3. Navigate to "API Keys" section
4. Click "Create new secret key"
5. Copy the API key (starts with `sk-`)
6. Add billing information (required for GPT-4 access)

**N8N Credential Setup:**
1. In N8N, go to "Credentials" → "Create New"
2. Search for "OpenAI"
3. Configure as follows:
   - **Credential Name**: `OpenAI GPT-4`
   - **API Key**: `[Your OpenAI API Key]`
   - **Organization ID**: (optional, leave blank unless you have one)
4. Save the credential

**API Configuration & Costs:**
- **Model**: GPT-4 (latest version)
- **Cost**: ~$0.03-0.06 per workflow execution
- **Rate Limits**: 3 requests per minute on free tier
- **Recommended**: Set usage alerts in OpenAI dashboard

#### 3. Gmail Integration Setup

**Gmail Account Configuration:**
1. Sign in to your Gmail account
2. Go to "Manage your Google Account"
3. Navigate to "Security" tab
4. Enable "2-Step Verification" (if not already enabled)
5. Go to "App passwords"
6. Generate app password for "Mail"
7. Copy the 16-character app password

**N8N Credential Setup:**
1. In N8N, go to "Credentials" → "Create New"
2. Search for "SMTP"
3. Configure as follows:
   - **Credential Name**: `Gmail SMTP`
   - **User**: `[your_email@gmail.com]`
   - **Password**: `[16-character app password]`
   - **Host**: `smtp.gmail.com`
   - **Port**: `587`
   - **Secure**: `true`
4. Save the credential

### Environment Variables

If using N8N self-hosted, set these environment variables in your `.env` file:

```bash
# OpenAI Configuration
OPENAI_API_KEY=sk-your_openai_api_key_here

# SerpAPI Configuration  
SERPAPI_KEY=your_serpapi_key_here

# Gmail Configuration (optional, if not using credentials)
GMAIL_USER=your_email@gmail.com
GMAIL_APP_PASSWORD=your_16_char_app_password
```

### Permission Scopes

#### SerpAPI Permissions
- **Search Results Access**: Automatically granted with API key
- **Rate Limiting**: Enforced server-side based on plan
- **Geographic Access**: Global by default

#### OpenAI API Permissions
- **Model Access**: GPT-4 access requires billing setup
- **Rate Limits**: Based on account tier and payment history
- **Usage Monitoring**: Available in OpenAI dashboard

#### Gmail Permissions
- **Send Email**: Granted through app-specific password
- **SMTP Access**: Enabled by default for app passwords
- **Security**: App passwords provide limited, specific access

### Workflow Node Configuration

#### Manual Trigger Node
```json
{
  "name": "Social Post Idea",
  "parameters": {
    "formTitle": "Social Media Content Generator",
    "formDescription": "Generate optimized content for all social platforms",
    "formFields": {
      "values": [
        {
          "fieldLabel": "Topic",
          "fieldType": "text",
          "requiredField": true
        },
        {
          "fieldLabel": "formMode", 
          "fieldType": "text",
          "requiredField": true
        },
        {
          "fieldLabel": "Link (optional)",
          "fieldType": "url",
          "requiredField": false
        },
        {
          "fieldLabel": "Keywords or Hashtags (optional)",
          "fieldType": "text", 
          "requiredField": false
        }
      ]
    }
  }
}
```

#### SerpAPI Node Configuration
```json
{
  "name": "SERP Research",
  "parameters": {
    "url": "https://serpapi.com/search",
    "method": "GET",
    "sendQuery": true,
    "queryParameters": {
      "parameters": [
        {
          "name": "q",
          "value": "={{ $json.Topic }}"
        },
        {
          "name": "engine", 
          "value": "google"
        },
        {
          "name": "num",
          "value": "10"
        }
      ]
    },
    "authentication": "predefinedCredentialType",
    "nodeCredentialType": "httpHeaderAuth"
  }
}
```

#### GPT-4 Content Generation Node
```json
{
  "name": "Social Media Content",
  "parameters": {
    "model": "gpt-4",
    "temperature": 0.7,
    "maxTokens": 4000,
    "prompt": "[Use the comprehensive prompt from the provided document]"
  }
}
```

#### Gmail Node Configuration
```json
{
  "name": "Gmail User for Approval",
  "parameters": {
    "operation": "send",
    "subject": "Your Social Media Content is Ready - {{ $json.Topic }}",
    "toEmail": "[recipient@email.com]",
    "fromEmail": "[your_email@gmail.com]",
    "html": "{{ $json.emailContent }}",
    "options": {
      "ccEmail": "",
      "bccEmail": "",
      "replyTo": "[your_email@gmail.com]"
    }
  }
}
```

### Troubleshooting Common Setup Issues

#### SerpAPI Issues
**Problem**: "Invalid API key" error
**Solution**: 
- Verify API key is correctly copied
- Check credential configuration in N8N
- Ensure API key is active in SerpAPI dashboard

**Problem**: Rate limit exceeded
**Solution**:
- Check usage in SerpAPI dashboard
- Upgrade plan if needed
- Add delay between requests

#### OpenAI API Issues
**Problem**: "Insufficient credits" error
**Solution**:
- Add billing information to OpenAI account
- Check usage limits in OpenAI dashboard
- Verify GPT-4 access is enabled

**Problem**: Rate limit errors
**Solution**:
- Reduce request frequency
- Check account tier limits
- Consider upgrading OpenAI account

#### Gmail Integration Issues
**Problem**: Authentication failed
**Solution**:
- Verify 2-step verification is enabled
- Generate new app-specific password
- Double-check SMTP settings

**Problem**: Emails not sending
**Solution**:
- Check Gmail security settings
- Verify app password is correctly entered
- Test SMTP connection separately

### Security Best Practices

#### API Key Management
- **Store Securely**: Use N8N credentials, never hardcode
- **Rotate Regularly**: Update API keys quarterly
- **Monitor Usage**: Set up alerts for unusual activity
- **Limit Permissions**: Use minimum required permissions

#### Email Security
- **App Passwords**: Never use main Gmail password
- **Access Monitoring**: Regularly review account access
- **Two-Factor Auth**: Keep 2FA enabled on Gmail account

### Performance Optimization

#### API Efficiency
- **Batch Requests**: Combine multiple operations where possible
- **Caching**: Consider caching SERP results for similar topics
- **Rate Limiting**: Respect API limits to avoid blocks

#### Cost Management
- **Monitor Usage**: Track API costs weekly
- **Set Limits**: Configure spending alerts
- **Optimize Prompts**: Reduce token usage where possible

### Scaling Considerations

#### Multi-User Setup
- **Credential Sharing**: Use team credentials for shared workflows
- **Access Control**: Implement proper user permissions
- **Cost Allocation**: Track usage per user/team

#### High-Volume Usage
- **API Upgrades**: Consider higher-tier plans
- **Load Balancing**: Distribute requests across time
- **Error Handling**: Implement retry logic for failed requests

### Support & Resources

#### Documentation Links
- **SerpAPI Docs**: [serpapi.com/docs](https://serpapi.com/docs)
- **OpenAI API Docs**: [platform.openai.com/docs](https://platform.openai.com/docs)
- **N8N Documentation**: [docs.n8n.io](https://docs.n8n.io)

#### Community Support
- **N8N Community**: [community.n8n.io](https://community.n8n.io)
- **OpenAI Community**: [community.openai.com](https://community.openai.com)

#### Emergency Contacts
- **API Issues**: Contact respective API support teams
- **Workflow Issues**: N8N community forum
- **Billing Issues**: Contact billing support for each service

This setup guide ensures you have all the necessary credentials, configurations, and troubleshooting knowledge to successfully implement and maintain the automated social media content generation workflow.
