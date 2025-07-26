# 🚀 Automated Lead Reactivation System

AI-powered lead reactivation system built with n8n that automatically re-engages "dead" leads for woodworking companies. This system intelligently filters leads, composes personalized outreach emails using Gemini AI, and tracks reactivation success.

## 📋 Project Overview

This system addresses the common challenge of reactivating leads who showed initial interest but never converted. It implements a two-workflow approach:

1. **Initial Outreach Workflow** - Identifies and contacts prime reactivation candidates
<img width="1484" height="796" alt="Automated Lead Reactivation   Outreach" src="https://github.com/user-attachments/assets/d232dcb6-d57b-49c5-9adc-c633a0aec55a" />


   
2. **Follow-up & Reactivation Workflow** - Manages responses and sends follow-up communications
<img width="1585" height="800" alt="Automated Lead Follow-up   Reactivation" src="https://github.com/user-attachments/assets/34c97bf0-b373-4fdf-a22d-86f0e2fcf037" />



## ✨ Key Features

### 🤖 AI-Powered Personalization
- **Gemini AI Integration** - Automatically composes personalized emails based on lead data
- **Smart Content Generation** - References specific services from lead notes (kitchen, deck, etc.)
- **Professional Tone** - Warm, non-pushy communication style

### 📊 Intelligent Lead Filtering
- **Multi-Criteria Filtering**:
  - Status = "open"
  - Older than 90 days
  - Not contacted in last 30 days
  - Not already re-engaged
- **Dynamic Date Calculations** - Uses n8n's Luxon library for accurate time filtering

### 🔄 Automated Follow-up System
- **2-Day Wait Period** - Respectful timing between communications
- **Response Detection** - Gmail trigger monitors for lead replies
- **Smart Status Updates** - Automatically tags re-engaged leads

### 📈 Comprehensive Tracking
- **Google Sheets Integration** - Real-time status updates and logging
- **Dashboard Reporting** - Tracks outreach and reactivation metrics
- **Error Handling** - Slack notifications for workflow issues

## 🏗️ System Architecture

### Workflow 1: Automated Lead Reactivation & Outreach
```
Start → Read Leads (Google Sheets) → Filter for Prime Leads → 
Compose Email (Gemini AI) → Format AI Output → Send Outreach Email → 
Log Outreach Status (Google Sheets)
```

### Workflow 2: Automated Lead Follow-up & Reactivation
```
Gmail Trigger → Get Leads (Google Sheets) → Filter for Prime Leads → 
Lead Responds? → [Yes: Log Reactivation] [No: Wait 2 Days → 
Compose 2nd Email → Send 2nd Outreach → Log Status]
```

## 🛠️ Technical Requirements

### Prerequisites
- **n8n Instance** (Cloud or Self-hosted)
- **Google Sheets Account** (for lead data and dashboard)
- **Gmail Account** (for sending emails and detecting replies)
- **Google Gemini API** (for AI email composition)
- **Slack Workspace** (for error notifications)

### Required Google Sheets Structure
```
lead_id | first_name | last_name | email | phone | source | 
date_created | status | last_contacted | notes | reengaged | reactivation_timestamp
```

## 🚀 Setup Instructions

### 1. Import Workflows
1. Import both JSON files into your n8n instance
2. Configure credentials for:
   - Google Sheets OAuth2
   - Gmail OAuth2
   - Google Gemini API
   - Slack API

### 2. Configure Google Sheets
1. Create a Google Sheet with the required column structure
2. Update the `documentId` and `sheetName` in both workflows
3. Create a "Dashboard" sheet for reporting

### 3. Update Workflow Parameters
1. **Google Sheets Nodes**: Update `documentId` and `sheetName` to match your sheet
2. **Gmail Nodes**: Verify sender email addresses
3. **Slack Nodes**: Update channel name for error notifications

### 4. Test the System
1. Run the "Automated Lead Reactivation & Outreach" workflow manually
2. Verify emails are sent and status is logged
3. Test the follow-up workflow with a sample reply

## 📊 Usage

### Initial Outreach
1. **Manual Trigger**: Start the first workflow
2. **Automatic Processing**: System filters leads and sends personalized emails
3. **Status Logging**: Outreach is logged with timestamp

### Follow-up Management
1. **Automatic Monitoring**: Gmail trigger detects replies
2. **Smart Routing**: Replied leads are marked as re-engaged
3. **Follow-up Emails**: Non-responders receive second outreach after 2 days

### Dashboard & Reporting
- **Outreach Tracking**: All sent emails are logged
- **Reactivation Metrics**: Successfully re-engaged leads are tracked
- **Error Monitoring**: Slack notifications for any workflow issues

## 🔧 Customization

### Email Personalization
The AI system analyzes the `notes` field to personalize content:
- **Kitchen-related**: References kitchen installation projects
- **Deck/Outdoor**: Mentions outdoor woodworking projects
- **General**: References custom woodworking projects

### Filtering Criteria
Modify the filtering logic in the Code nodes:
```javascript
// Current criteria
if (lead.status !== 'open') continue;
if (!(created < ninetyDaysAgo.toJSDate())) continue;
if (lastContacted && lastContacted >= thirtyDaysAgo.toJSDate()) continue;
if (lead['reengaged '] === 'Yes') continue;
```

### Timing Adjustments
- **Wait Period**: Modify the "Wait 2 Days" node for different follow-up timing
- **Filtering Windows**: Adjust the 90-day and 30-day windows as needed

## 📈 Performance Metrics

### Success Indicators
- **Reactivation Rate**: Percentage of leads who respond
- **Response Time**: Average time from outreach to reply
- **Follow-up Success**: Conversion rate of second outreach

### Monitoring
- **Error Rate**: Track workflow failures via Slack
- **Email Delivery**: Monitor Gmail sending success
- **Data Quality**: Ensure lead data completeness

## 🔒 Security & Compliance

### Data Protection
- **OAuth2 Authentication**: Secure API access
- **No Data Storage**: All data remains in Google Sheets
- **Audit Trail**: Complete logging of all actions

### Email Compliance
- **Unsubscribe Options**: Include in email templates
- **GDPR Compliance**: Respect data privacy regulations
- **Professional Communication**: Non-spam, value-focused content

## 🚀 Deployment Options

### Production Setup
1. **Scheduled Execution**: Set up cron triggers for regular outreach
2. **Error Monitoring**: Configure Slack alerts for 24/7 monitoring
3. **Backup Strategy**: Regular exports of workflow configurations

### Scaling Considerations
- **Batch Processing**: Handle large lead lists efficiently
- **Rate Limiting**: Respect API limits for Gmail and Gemini
- **Data Validation**: Ensure lead data quality before processing

## 🤝 Support & Maintenance

### Regular Maintenance
- **Credential Rotation**: Update OAuth tokens as needed
- **API Monitoring**: Track usage limits and performance
- **Data Cleanup**: Archive old lead data periodically

### Troubleshooting
- **Common Issues**: Check credential validity and API quotas
- **Error Logs**: Review Slack notifications for workflow issues
- **Data Validation**: Verify Google Sheets structure and permissions

## 📝 License

This project is provided as-is for educational and commercial use. Please ensure compliance with all applicable laws and regulations regarding email marketing and data privacy.

## 🙏 Acknowledgments

- **n8n Team** - For the powerful automation platform
- **Google Gemini** - For AI-powered content generation
- **Google Sheets** - For reliable data storage and collaboration

---

**Built with ❤️ for efficient lead reactivation and business growth**
