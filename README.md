WorxTimer - README
================

A lightweight, client‑side timer and user/event tracker built with plain HTML, Bootstrap 5, and localStorage.

**Table of contents**

1.  [Overview](#overview)
2.  [Features](#features)
3.  [Quick start](#quickstart)
4.  [Usage](#usage)
5.  [Data model](#data-model)
6.  [Configuration](#configuration)
7.  [Optional PWA & mobile (Capacitor)](#pwa-mobile)
8.  [Project structure](#project-structure)
9.  [Tech stack](#tech-stack)
10.  [Troubleshooting](#troubleshooting)
11.  [License](#license)

Overview
--------

This project provides a simple in‑browser timer for user‑specific events. After passing a PIN gate, you can manage _Users_, assign _Events_ to each user, and start/stop/reset a millisecond timer per event. All data persists to `localStorage`; no server is required.

**Default PIN**

The app shows a PIN screen first. The default PIN is `3030`. You can change it in `www/index.html` (see [Configuration](#configuration)).

**Persistence**

Users & events are stored under the localStorage key `pwa-users` in the current browser only.

Features
--------

*   🔐 PIN gate before accessing the app (default `3030`).
*   👤 **User management**: add, edit, delete users with fields: Name, Employee ID, Department, Manager.
*   🗂️ **User search** box to quickly filter users.
*   📝 **Event management** per user: add/delete events with a title and tracked time.
*   🔎 **Event search** within a selected user.
*   ⏱️ **Timer** view with Start / Stop / Reset and **Save** actions (elapsed time updates live).
*   💾 Automatic persistence to `localStorage`.
*   💄 Built with Bootstrap 5 + Bootstrap Icons for clean UI.

Quick start
-----------

1.  Open `www/index.html` in any modern browser, or serve the `www/` folder with a static web server.
2.  Enter the PIN (`3030` by default).
3.  Add a user, then add events for that user.
4.  Open an event and use the timer controls.

Tip: for best results when testing locally, serve via HTTP (e.g., Python or Node) to avoid any browser restrictions.

### Serve with Python

    cd www
    python -m http.server 5173

### Serve with Node

    npm -g i serve
    serve www -l 5173

Usage
-----

1.  **Unlock**: enter the access PIN.
2.  **Users** page:
    *   Use the _+_ button to add a user (Name, Employee ID, Department, Manager).
    *   Search using the field above the list.
    *   Edit or delete users via the action buttons on each row.
    *   Select a user to view their events.
3.  **Events** page (for a specific user):
    *   Use the _+_ button to add a new event title.
    *   Search events by title.
    *   Delete events you no longer need.
    *   Click an event row to open the _Timer_ view.
4.  **Timer** page:
    *   Start begins counting (precision: 10 ms).
    *   Stop pauses the timer.
    *   Reset sets the elapsed time back to zero.
    *   Save persists the current elapsed time to storage.

The displayed format is `MM:SS.hh` (minutes, seconds, hundredths).

Data model
----------

### User

    {
      "name": "Alice Smith",
      "employeeId": "E12345",
      "department": "Operations",
      "manager": "J. Doe",
      "events": [ … ]
    }

### Event

    {
      "title": "Shift A",
      "elapsed": 123456  // milliseconds
    }

All users are stored in `localStorage` under the key `pwa-users` as a JSON array.

Configuration
-------------

Setting

Where

Notes

Access PIN

`www/index.html`

Search for `const appPin = "3030"` and change the value.

Storage key

`www/index.html`

Search for `const dbKey = "pwa-users"` if you want a different key.

Branding/UI

`www/index.html`

Update the title, styles, or Bootstrap classes as desired.

Optional: PWA & mobile packaging (Capacitor)
--------------------------------------------

The repository includes Capacitor dependencies in `package.json`, but a full Capacitor setup (e.g., `capacitor.config.ts`, native platform projects) is not yet present. If you want mobile builds:

1.  Install dependencies: `npm i`
2.  Initialize Capacitor in the project root:
    
        npx cap init "User Timer App" "com.example.timer"
    
3.  Set the web directory to `www` in `capacitor.config.ts`:
    
        export default {
          appId: "com.example.timer",
          appName: "User Timer App",
          webDir: "www",
          bundledWebRuntime: false
        };
    
4.  Add platforms and copy web assets:
    
        npx cap add android
        npx cap add ios
        npx cap copy
    
5.  Open the native projects to build/run:
    
        npx cap open android
        npx cap open ios
    

Note: You may also add a proper `manifest.json` and service worker to enable installable PWA behavior in browsers.

Project structure
-----------------

    timer/
    ├─ package.json
    ├─ package-lock.json
    ├─ node_modules/           # Capacitor & build-time deps (optional for static hosting)
    └─ www/
       └─ index.html           # Entire app (UI + logic)

Tech stack
----------

*   HTML5 + vanilla JavaScript
*   Bootstrap 5 + Bootstrap Icons (via CDN)
*   localStorage for persistence
*   (Optional) Capacitor for mobile packaging

Troubleshooting
---------------

*   **PIN never unlocks:** Ensure you entered the configured PIN (default `3030`). If you changed it in code, clear browser cache and refresh.
*   **Changes not saved:** Verify your browser allows localStorage. Try a different browser profile or clear site data.
*   **Timer seems inaccurate:** The UI updates in ~10 ms increments. Background tabs or mobile power‑saving modes can throttle timers.
*   **Mobile builds fail:** Complete the Capacitor setup steps and ensure Android Studio / Xcode are installed and up to date.