---
title: "Chrome extension to start a new Clockify time entry"
excerpt: "
  When I'm developing outside of working time, I like to collect addresses of visited websites.
  After a long time of collecting it by hand, I decided to change something.
  With Chrome Extension, I can do it automatically!
  "
date: 2021-04-28 08:30:00 +01:00
last_modified_at: 2021-04-28 08:30:00 +01:00
tags:
  - learning
  - management
  - code
  - automation
---

  When I'm developing outside of working time, I like to collect addresses of visited websites.
  I also collect how much time I spend on each website.
  After a long time of collecting it by hand, I decided to change something.
  Automation has overtaken me!

  I decided to prepare a **Chrome extension, allowing me to add a new time entry to Clockify automatically**.
  It should eliminate forgetting to start new entries and reduce switching between windows.

### Why Clockify

  Clockify allows me to manage projects and work time.
  It is a perfect choice, especially for private projects.
  By using the extension for macOS, **I can manage my work time**.
  Thanks to their [API](https://clockify.me/developers-api), I can quickly check and manipulate entities.

### Why Chrome Extension

  It takes time to save a website address or title each time.
  **Even though it takes a few seconds, minutes are already accumulating with dozens of websites visited.**
  I also forgot to add a link many times.

  An additional motivation was the desire to **learn how to create Chrome extensions**.

## Code

  I decided to use the official documentation and introduction: [Getting started](https://developer.chrome.com/docs/extensions/mv3/getstarted/).
  Finally, I got the following code.
  **You can use it by saving all files according to the given names under one directory.**
  Then add a new extension to your browser.

  ```json
  /* manifest.json */
  {
    "name": "Clockify auto-sync",
    "description": "Capture the current URL and start a new time entry in Clockify via API.",
    "version": "1.0",
    "manifest_version": 3,
    "permissions": ["tabs", "scripting"],
    "action": {
      "default_popup": "popup.html",
      "default_icon": {
        "16": "/icon.png",
        "32": "/icon.png",
        "48": "/icon.png",
        "128": "/icon.png"
      }
    },
    "icons": {
      "16": "/icon.png",
      "32": "/icon.png",
      "48": "/icon.png",
      "128": "/icon.png"
    }
  }
  ```

  `name`, `description`, and `version` indicate basic information about the extension, which you can see on the extensions list.
  I also used the latest (v.3) version (defined by `manifest_version`).
  In the `permissions` you can see what the extension requires to work correctly.
  **It does not use any background services as the action is only executed after pressing the button** on the view specified in `default_popup`.

  ```html
  <!-- popup.html -->
  <!DOCTYPE html>
  <html>
    <head>
      <link rel="stylesheet" href="button.css">
    </head>
    <body>
      <button id="startEntry">Start new time entry</button>
      <script src="popup.js"></script>
    </body>
  </html>
  ```

  ```js
  // popup.js

  let startEntryButton = document.getElementById("startEntry");

  startEntryButton.addEventListener("click", async () => {
    let [tab] = await chrome.tabs.query({
      active: true,
      currentWindow: true
    });

    var headers = new Headers();
    headers.append("X-Api-Key", "SECRET_TOKEN");
    headers.append("Content-Type", "application/json");

    var raw = JSON.stringify({
      "start": new Date().toISOString(),
      "projectId": "PROJECT_ID",
      "description": tab.title + ' ' + tab.url
    });

    var requestOptions = {
      method: 'POST',
      headers: headers,
      body: raw,
      redirect: 'follow'
    };

    fetch("https://api.clockify.me/api/v1/workspaces/WORKSPACE_ID/time-entries", requestOptions)
      .then(response => response.text())
      .then(result => {
        startEntryButton.innerText = "Started";
        startEntryButton.style.background = "#4caf50";
      })
      .catch(error => {
        startEntryButton.innerText = "ERROR!";
        startEntryButton.style.background = "#e91e63";
        console.log('error', error);
      });
  });
  ```

  I prepared the code for adding new data in [Postman](https://www.postman.com/).
  Next, I exported the action as JavaScript - fetch.
  `SECRET_TOKEN`, `PROJECT_ID`, `WORKSPACE_ID` are data from Clockify.
  For safety, I have removed them from the script.
  It can be done better for sure, but it is enough for my needs.

## Summary

  This simple extension took about an hour of work.
  The joy, however, was excellent.
  **A simple extension allowed me to reduce switching between browser tabs or application windows.**
  Two clicks, and the clock is running.
  **This automation is great!**
