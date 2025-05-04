# Weekly Meeting Summary Generator (Google Apps Script)

## Overview

This Google Apps Script automates the process of generating a weekly meeting summary. It fetches upcoming events from your Google Calendar for the next seven days, filters out irrelevant meetings based on predefined criteria, and inserts a structured summary into a designated Google Document.

## Features

*   Fetches calendar events for the upcoming 7 days.
*   Filters events based on:
    *   Specific keywords or phrases in the event title (e.g., 'home', 'stand-up', 'internal').
    *   Keywords included in the title.
    *   Placeholder meetings created by the user for themselves.
    *   Meetings organized by specific attendees or domains
    *   Etc.
*   Groups the filtered events by date.
*   Formats the summary with dates as bold headings and placeholders for meeting details (Account/Time, Attendees, Topics, Outcome).
*   Inserts the generated summary into a specified Google Document, placing it after the first Heading 1 element found in the document.

## How to Use

1.  **Copy the Script:**
    *   Open the Google Apps Script editor (you can do this from Google Drive by clicking `New` > `More` > `Google Apps Script`, or by opening a Google Sheet/Doc and going to `Tools` > `Script editor`).
    *   Copy the entire content of the `Weekly Meeting Summary - Live` file and paste it into the editor, replacing any default code.
    *   Save the script project (e.g., "Weekly Meeting Summary").

2.  **Configure the Target Document:**
    *   Open the Google Doc where you want the summary to be inserted.
    *   Find the Document ID in the URL. It's the long string of characters between `/d/` and `/edit`. For example, in `https://docs.google.com/document/d/14ZC3jrRXGCK0hfgBiN92fItkk8B1FDhCKgDLwWYBYUQ/edit`, the ID is `14ZC3jrRXGCK0hfgBiN92fItkk8B1FDhCKgDLwWYBYUQ`.
    *   In the script editor, find the line:
        ```javascript
        const docId = 'YOUR_DOCUMENT_ID_HERE'; // Replace with your actual Document ID
        ```
    *   Replace `'YOUR_DOCUMENT_ID_HERE'` with the actual ID of your Google Doc. Make sure the ID remains enclosed in single quotes. *Note: The current script uses `'14ZC3jrRXGCK0hfgBiN92fItkk8B1FDhCKgDLwWYBYUQ'`.*

3.  **Grant Permissions:**
    *   The first time you run the script (or set up a trigger), Google will ask for authorization to access your Calendar and Docs.
    *   Review the permissions carefully and grant access if you trust the script. It needs access to read your calendar events and edit your Google Document.

4.  **Run the Script:**
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

*   **Filtering:** You can modify the `excludedTitles` array in the script to add or remove keywords for filtering events. You can also adjust the other filtering conditions (e.g., organizer domain, attendee checks) within the `events.forEach` loop if needed.
*   **Output Format:** Change the text and formatting within the `newContent.push` lines in the latter part of the script to alter how the summary appears in the Google Doc.

## Important Notes

*   This script requires access to your Google Calendar and Google Docs data. Ensure you understand the permissions you are granting.
*   The script inserts content into the specified Google Doc. Make sure the `docId` is correct to avoid modifying the wrong document.
*   The script relies on finding a Heading 1 style element in the target document to determine where to insert the summary. If no Heading 1 is found, it will insert the summary at the very beginning of the document body.
