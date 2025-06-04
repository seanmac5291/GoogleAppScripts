# Google Apps Script Tools Collection

This repository contains two powerful Google Apps Script tools for automating common business tasks:

1. **Weekly Meeting Summary Generator** - Automatically creates structured meeting summaries from your Google Calendar
2. **Other Contacts Exporter** - Exports "Other Contacts" from Google Contacts to Google Sheets with pagination support

---

## 📅 Weekly Meeting Summary Generator

### Overview

Automatically generates a weekly meeting summary from your Google Calendar events for the next 7 days, filters out irrelevant meetings, and pushes a structured markdown summary to a GitHub repository.

### Features

- Fetches calendar events for the upcoming 7 days
- Smart filtering to exclude:
  - Personal events (home, personal appointments)
  - Internal meetings (stand-ups, all hands)
  - Specific keywords and patterns
  - Single-attendee meetings created by you
- Groups events by date with clean formatting
- Pushes to GitHub with automatic file creation/updating
- SAML-aware GitHub integration

### Setup Instructions

#### 1. Create the Google Apps Script Project

1. Go to [Google Apps Script](https://script.google.com)
2. Click "New Project"
3. Copy the entire content from `Weekly Meeting Summary - Live` file
4. Paste it into the editor (replacing default code)
5. Save with a descriptive name (e.g., "Weekly Meeting Summary")

#### 2. Configure GitHub Integration

1. **Create GitHub Personal Access Token:**
   - Go to GitHub Settings → Developer settings → Personal access tokens
   - Click "Generate new token (classic)"
   - Select scopes: `repo` (full control of private repositories)
   - Copy the generated token

2. **Add Token to Apps Script:**
   - In Apps Script editor, click Project Settings (gear icon)
   - Go to "Script properties" tab
   - Click "Add script property"
   - Name: `GITHUB_TOKEN`
   - Value: `your-github-personal-access-token`
   - Click "Save"

3. **Handle SAML Organizations (if applicable):**
   - If targeting a SAML-enforced organization (like github/SLGEast):
   - Go back to your GitHub token settings
   - Find your token and click on it
   - Scroll to "Organization access"
   - Click "Authorize" for the target organization
   - Complete SAML authentication if prompted

#### 3. Configure Target Repository

Update these variables in the script if needed:

```javascript
const owner = 'github';           // GitHub username/organization
const repo = 'SLGEast';          // Repository name
const path = 'WeeklyMeetingSummary.md';  // Target file path
```

#### 4. Set Up Automation

1. **Grant Calendar Permissions:**
   - Run the `createNextSevenDaysSummary` function once manually
   - Authorize calendar access when prompted

2. **Create Weekly Trigger:**
   - Click "Triggers" (clock icon) in the left sidebar
   - Click "+ Add Trigger"
   - Choose function: `createNextSevenDaysSummary`
   - Event source: `Time-driven`
   - Type: `Week timer`
   - Day: `Every Monday` (or preferred day)
   - Time: `9am to 10am` (or preferred time)
   - Click "Save"

### Usage

- **Manual Run:** Select `createNextSevenDaysSummary` and click "Run"
- **Automatic:** Set up weekly trigger for hands-off operation
- **Monitor:** Check "Executions" tab for run history and logs

### Customization

Modify the `excludedTitles` array to add/remove filtered keywords:

```javascript
const excludedTitles = [
  'home', 'personal', 'stand-up', 'internal',
  // Add your own keywords here
];
```

---

## 📧 Other Contacts Exporter

### Overview

Exports your Google "Other Contacts" (people you've interacted with via email but haven't explicitly saved) to a Google Sheet. Now includes **pagination support** to handle large contact lists (3000+ contacts).

### Features

- **Complete Contact Export:** Names, emails, phones, companies, notes
- **Pagination Support:** Handles thousands of contacts across multiple API pages
- **Smart Filtering:** Excludes specified domains (like github.com)
- **Multiple Export Methods:** Tries different APIs for maximum compatibility
- **Auto-formatting:** Creates organized spreadsheet with headers and metadata
- **Weekly Automation:** Set up automatic exports
- **Detailed Logging:** Progress tracking and error handling

### Setup Instructions

#### 1. Create Google Apps Script Project

1. Go to [Google Apps Script](https://script.google.com)
2. Click "New Project"
3. Copy the entire content from `ContactScraping` file
4. Paste into editor and save with name like "Contact Exporter"

#### 2. Configure Export Settings

**Optional - Use Existing Spreadsheet:**
```javascript
const SPREADSHEET_ID = '1W-0n5y43H4qR6-TGJZ3sErNrFiTs8YDFmEmwai6lVzc'; // Your sheet ID
```
Leave empty (`''`) to auto-create a new spreadsheet.

**Domain Filtering:**
```javascript
const EXCLUDED_DOMAINS = [
  'github.com',
  'company.com',    // Add domains to exclude
  'internal.org'
];
```

#### 3. Grant Permissions

1. Run `testExport` function first
2. Click "Review permissions" → "Go to [Project Name]" → "Allow"
3. Grant access to:
   - Google Contacts (read contacts)
   - Google Sheets (create/update spreadsheets)

#### 4. Set Up Automation (Optional)

Run `setupWeeklyTrigger` to create automatic weekly exports every Monday at 9 AM.

### Usage

#### Available Functions

- **`testExport`** - Test run with detailed logging (recommended first)
- **`exportOtherContactsToSheet`** - Full export to spreadsheet
- **`saveAndExportContacts`** - Save other contacts as regular contacts, then export
- **`setupWeeklyTrigger`** - Enable weekly automation
- **`removeAllTriggers`** - Disable automation

#### Running an Export

1. **First Time:** Run `testExport` to verify everything works
2. **Regular Use:** Run `exportOtherContactsToSheet` for full export
3. **Large Lists:** The script automatically handles pagination

#### What You'll See

```
Starting Other Contacts export...
Attempting to access Other Contacts using otherContacts.list...
Fetching page 1...
Page 1: Found 1000 contacts (Total so far: 1000)
Fetching page 2...
Page 2: Found 1000 contacts (Total so far: 2000)
Fetching page 3...
Page 3: Found 725 contacts (Total so far: 2725)
✅ otherContacts.list pagination complete!
📧 Total other contacts found across all pages: 2725
After domain filtering: 2650 other contacts
```

### Exported Data

| Column | Description |
|--------|-------------|
| Full Name | Complete contact name |
| First Name | Given name |
| Last Name | Family name |
| Primary Email | Main email address |
| All Emails | All emails (comma-separated) |
| Primary Phone | Main phone number |
| All Phones | All phones with labels |
| Company | Organization name |
| Job Title | Position title |
| Notes | Contact notes/biography |
| Last Updated | Last modification date |

### Understanding "Other Contacts"

Google Contacts categories:
- **Contacts:** People you've explicitly added
- **Other Contacts:** People from email interactions you haven't explicitly saved

The script targets "Other Contacts" including:
- Contacts in the "Other Contacts" group
- Contacts without any group assignment
- Auto-added contacts from email interactions

### Troubleshooting

#### Common Issues

**"No contacts found":**
- Verify you have "Other Contacts" in Google Contacts
- Check authorization permissions
- Try running `testExport` first

**"Pagination stopped early":**
- Normal behavior when all contacts are retrieved
- Check final contact count in logs

**"Permission denied":**
- Re-authorize the script
- Verify you're using the correct Google account

**"Spreadsheet errors":**
- Check `SPREADSHEET_ID` is correct
- Leave empty to auto-create new sheet

#### Viewing Detailed Logs

1. Go to Apps Script → "Executions"
2. Click on any execution to see full logs
3. Look for pagination progress and final counts

### Advanced Features

#### Custom Filtering

Modify `shouldExcludeContact` function to add custom filtering logic:

```javascript
function shouldExcludeContact(contact) {
  // Add your custom filtering logic here
  // Return true to exclude, false to include
}
```

#### Different Export Schedules

Modify trigger timing:

```javascript
// Daily at 8 AM
ScriptApp.newTrigger('exportOtherContactsToSheet')
  .timeBased()
  .everyDays(1)
  .atHour(8)
  .create();
```

---

## 🔐 Security & Privacy

- **Data Privacy:** All processing happens within Google's secure environment
- **No External Servers:** Data never leaves Google/GitHub ecosystem
- **Token Security:** GitHub tokens should have minimal required permissions
- **Revocable Access:** Permissions can be revoked anytime via Google Account settings

## 🐛 Support & Troubleshooting

### General Debugging Steps

1. **Check Execution Logs:** Apps Script → Executions → Click on run
2. **Verify Permissions:** Re-authorize if needed
3. **Test Functions:** Use `testExport` or manual runs first
4. **Check Quotas:** Google Apps Script has daily execution limits

### Common Error Solutions

- **GitHub 403 Forbidden:** Authorize token for SAML organizations
- **Calendar/Contacts access denied:** Re-run authorization flow
- **Trigger failures:** Check trigger status and error notifications
- **Large dataset timeouts:** Script includes automatic pagination and chunking

### Getting Help

1. Review execution logs for specific error messages
2. Check Google Apps Script quota limits
3. Verify API permissions and tokens
4. Test with smaller datasets first

---

## 📝 License & Usage

These scripts are provided as-is for personal and professional use. Please review and understand the permissions you're granting before use.
