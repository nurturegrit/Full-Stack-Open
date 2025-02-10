## 0.4: New note diagram

```mermaid

sequenceDiagram
    participant browser
    participant server

    Note right of browser: A User submits a Form from their browser to `post` a new_note
    browser->>server: POST https://studies.cs.helsinki.fi/exampleapp/new_note
    activate server
    Note left of server: the new_note data is pushed to the `notes` (list of all notes) in the server (not stored in a DB)
    server ->> browser: Response Status Code`302`- Server asks browser to make a new GET request to address in Header's location.
    deactivate server

    Note right of browser: Browser makes a a new GET request as per Response Status 302 which results in a series of GET requests
    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/notes
    activate server
    server-->>browser: HTML document
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.css
    activate server
    server-->>browser: the css file
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.js
    activate server
    server-->>browser: the JavaScript file
    deactivate server

    Note right of browser: The browser starts executing the JavaScript code that fetches the JSON from the server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/data.json
    activate server
    server-->>browser: [{ "content": "HTML is easy", "date": "2023-1-1" }, ... {"content":'new note', "date": 'date of today' ]
    deactivate server

    Note right of browser: The browser executes the callback function that renders the notes

```

## 0.5: Single page app diagram

```mermaid

sequenceDiagram
    participant browser
    participant server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/spa
    activate server
    server-->>browser: HTML document
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.css
    activate server
    server-->>browser: the css file
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/spa.js
    activate server
    server-->>browser: the JavaScript file
    deactivate server

    Note right of browser: The browser starts executing the JavaScript code that fetches the JSON from the server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/data.json
    activate server
    server-->>browser: [{ "content": "HTML is easy", "date": "2023-1-1" }, ... ]
    deactivate server

    Note right of browser: Fetched JSON data (notes), is displayed on page by adding HTML elements to the page using the DOM-API

```

## 0.6: New note in Single page app diagram

```mermaid

sequenceDiagram
    participant browser
    participant server

    Note right of browser: A User submits a new note, the default submit action is prevented by Javascript code
    Note right of browser: JavaScript Code (already fetched from server) adds the new note submitted by user to the list of notes. (Without Reload)
    Note right of browser: Javascript code sends the new note as Json Data to the server with Content Type as application/json in the Request Header
    browser->>server: POST https://studies.cs.helsinki.fi/exampleapp/new_note_spa
    activate server
    Note left of server: The Server takes the new note json data and pushes the new note into the notes list.
    deactivate server

```

**It would have been better if the Javascript first POSTed the new note json data to the server, and upon a success message Response
, a new note is rendered on the page.**