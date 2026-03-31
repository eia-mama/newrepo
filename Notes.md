# Creating a New Note - Sequence Diagram

This diagram shows the interaction between the user, browser client, and server when creating a new note on https://studies.cs.helsinki.fi/exampleapp/notes

## New Note Creation Flow

```mermaid
sequenceDiagram
    participant User
    participant Browser
    participant Server

    User->>Browser: 1. Launch URL
    activate Browser
    Note over Browser: Navigate to https://studies.cs.helsinki.fi/exampleapp/notes

    Browser->>Server: GET /exampleapp/notes
    activate Server
    Server-->>Browser: HTML response with references to:<br/>- /exampleapp/main.css<br/>- /exampleapp/main.js
    deactivate Server

    Browser->>Server: GET /exampleapp/main.css
    activate Server
    Server-->>Browser: CSS stylesheet
    deactivate Server

    Browser->>Server: GET /exampleapp/main.js
    activate Server
    Server-->>Browser: JavaScript code
    deactivate Server

    Browser->>Browser: 2. Execute JavaScript:<br/>Load initial page with form

    Browser->>Server: 3. GET /exampleapp/data.json
    activate Server
    Server-->>Browser: JSON array of existing notes
    deactivate Server

    Browser->>Browser: Render notes on page
    Browser-->>User: Display page with note list & form
    deactivate Browser

    User->>Browser: 4. Type note content in text field
    User->>Browser: Click "Save" button
    activate Browser

    Browser->>Server: POST /exampleapp/new_note
    activate Server
    Note over Server: Add new note to data.json<br/>(write to file on server)
    Server-->>Browser: Redirect or response
    deactivate Server

    Browser->>Server: 5. GET /exampleapp/notes (page reload)
    activate Server
    Server-->>Browser: Updated HTML response
    deactivate Server

    Note over Browser: Same sequence repeats (steps 1-4)
    Browser->>Server: GET /exampleapp/main.css
    Browser->>Server: GET /exampleapp/main.js
    Browser->>Server: GET /exampleapp/data.json (now with new note)

    Browser->>Browser: Render updated page
    Browser-->>User: Display page with new note added
    deactivate Browser
```

## Sequence Breakdown

1. **Initialize Page**: User launches the URL, server responds with HTML containing CSS and JS references
2. **Load Resources**: Browser loads the CSS stylesheet and JavaScript code
3. **Load Data**: JavaScript makes XHR/Fetch request to get existing notes from data.json
4. **Save Note**: User enters note content and clicks Save, triggering POST to /exampleapp/new_note
   - Server appends the new note to the data.json file
   - Server responds with redirect/confirmation
5. **Reload**: Page reloads, repeating the initial sequence
   - Browser requests data.json again, which now includes the newly saved note
   - Updated note list is displayed to the user

---

**Note**: The example app uses a simple file-based storage (data.json) instead of a database for storing notes.

