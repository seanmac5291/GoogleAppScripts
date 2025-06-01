# Weekly Meeting Summary Generator for GitHub

## Overview

This Google Apps Script automates the process of generating a weekly meeting summary. It fetches upcoming events from your Google Calendar for the next seven days, filters out irrelevant meetings based on predefined criteria, and pushes a structured markdown summary to a GitHub repository.

## Features

*   Fetches calendar events for the upcoming 7 days.
*   Filters events based on:
    *   Specific keywords or phrases in the event title (e.g., 'home', 'stand-up', 'internal').
    *   Keywords included in the title.
    *   Placeholder meetings created by the user for themselves.
    *   Meetings organized by specific attendees or domains
    *   Etc.
*   Groups the filtered events by date.
*   Formats the summary in Markdown with dates as headings and placeholders for meeting details (Account/Time, Attendees, Topics, Outcome).
*   Pushes the generated summary to a GitHub repository, prepending it to the top of the existing file (WeeklyMeetingSummary.md).

## How to Use

1.  **Copy the Script:**
    *   Open the Google Apps Script editor (you can do this from Google Drive by clicking `New` > `More` > `Google Apps Script`, or by opening a Google Sheet/Doc and going to `Tools` > `Script editor`).
    *   Copy the entire content of the `Weekly Meeting Summary - Live` file and paste it into the editor, replacing any default code.
    *   Save the script project (e.g., "Weekly Meeting Summary").

2.  **Configure GitHub Access:**
    *   You'll need a GitHub Personal Access Token with appropriate permissions to push to the target repository.
    *   In the script editor, click on `Project Settings` (gear icon).
    *   Go to the `Script Properties` tab and add a new property:
        *   Name: `GITHUB_TOKEN`
        *   Value: `your-github-personal-access-token`
    *   Click `Save`.
    *   **IMPORTANT**: If the target organization has SAML enforcement enabled (like the github organization), you must authorize your token for that organization:
        *   Go to your GitHub Settings → Developer settings → Personal access tokens
        *   Find your token and click on it
        *   Scroll down to "Organization access" 
        *   Find the github organization and click "Authorize"
        *   You may be prompted to confirm access with your SAML credentials

3.  **Configure Repository Settings (if needed):**
    *   The script is currently configured to push to the 'github/SLGEast' repository.
    *   If you need to change the target repository or file path, modify these lines in the script:
        ```javascript
        const owner = 'github';
        const repo = 'SLGEast';
        const path = 'WeeklyMeetingSummary.md';
        ```

4.  **Grant Permissions:**
    *   The first time you run the script (or set up a trigger), Google will ask for authorization to access your Calendar.
    *   Review the permissions carefully and grant access if you trust the script.

5.  **Run the Script:**
    *   **Manually:** Select the `createNextSevenDaysSummary` function from the dropdown menu in the script editor toolbar and click the "Run" (play) button.
    *   **Automatically (Trigger):**
        *   Click on the "Triggers" (clock) icon in the left sidebar of the script editor.
        *   Click "+ Add Trigger".
        *   Configure the trigger settings:
            *   Choose which function to run: `createNextSevenDaysSummary`
            *   Choose which deployment should run: `Head`
            *   Select event source: `Time-driven`
            *   Select type of time based trigger: `Weekly timer` (or adjust as needed, e.g., every Monday morning).
            *   Set error notification settings.
        *   Click "Save". You might need to authorize the script again for triggers.

## Customization

*   **Filtering:** You can modify the `excludedTitles` array in the script to add or remove keywords for filtering events. You can also adjust the other filtering conditions (e.g., organizer domain, attendee checks) within the `events.forEach` loop.
*   **Output Format:** Change the markdown formatting in the script to alter how the summary appears in the GitHub file.
*   **Commit Message:** You can customize the commit message by modifying the `message` property in the `updateGitHubFile` function.

## Important Notes

*   This script requires access to your Google Calendar data. Ensure you understand the permissions you are granting.
*   The script pushes content to a GitHub repository using a personal access token. Keep this token secure and consider using a token with the minimal necessary permissions.
*   If the target file (`WeeklyMeetingSummary.md`) doesn't exist in the repository, the script will create it automatically.
*   Each time the script runs, it prepends the new weekly summary to the top of the existing file, maintaining a chronological record with the most recent entries at the top.
