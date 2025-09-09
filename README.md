# 📘 README

## Problem 
The process of generating anniversary and birthday greeting posts is currently done manually. This entire process, including the effective operational time and coordination time, can take up to 5 days. Each week, a summary of upcoming anniversaries and birthdays is sent to the manager from Salesforce. After that, the manager and the designer work together to create the artwork, define the message, and manually publish the posts to the celebrations channel. Automating this process with N8N and an AI node significantly reduces the time and level of supervision needed from the manager, while still fostering a celebratory spirit within the team.
- High manual workload and dependency on individuals
- Lack of timeliness and scalability
- Inefficient use of creative and managerial resources instead of focusing on higher-impact activities.

## Users  
- **General Staff**: see celebratory posts in Slack channels.  
- **HR Team**: receive a daily private summary message in a dedicated channel.  
- **Admins**: maintain Salesforce connection, Slack credentials, and n8n workflow.  

## Setup  
1. **Salesforce**:  
   - Node `Select all Contacts with birthdays or work anniversaries`.  
   - SOQL adjusted to fetch all active employees.  

2. **Calendar Filter (Code Node)**:  
   - On Monday: emits Sat+Sun (retro) **plus** Monday itself.  
   - On Tue–Fri: emits only “today”.  

3. **Dual Output**:  
   - **Output A** → `Prepare AI Prompt` → Slack public posts (with Pollinations images).  
   - **Output B** → `Collect for HR` → `Prepare Summary for HR` → Slack HR channel.  

4. **Prepare Slack Blocks**:  
   - Anniversary posts avoid cake/pizza; birthdays can include.  
   - Images styled with geometric corporate palette.  

5. **Prepare Summary for HR**:  
   - Always triggered if there are items.  
   - Distinguishes “Today” vs “Weekend catch-up”.  
   - Skips empty messages.  

## Env / Config  
- **Environment Variables**:  
  - `SALESFORCE_AUTH` → OAuth / login creds for n8n Salesforce node.  
  - `SLACK_TOKEN` → Bot/User token for Slack.  
  - `SLACK_HR_CHANNEL` → ID of HR channel (private).  
  - `TZ` → e.g. `America/Managua` (adjustable).  

- **Optional**:  
  - AI model settings (temperature, top_p, etc.) can be passed as workflow variables.  

## Security Notes  
- Salesforce + Slack credentials are stored in **n8n Credentials** (never inline in workflow JSON).  
- AI model prompts avoid exposing PII beyond what’s needed (name, title, hire date, food preferences).  
- Food/allergy data is only used for **birthdays** and excluded for anniversaries.  
- Logs should be sanitized before sharing externally.  
- HR summaries include only celebrants’ names, not sensitive details.  
