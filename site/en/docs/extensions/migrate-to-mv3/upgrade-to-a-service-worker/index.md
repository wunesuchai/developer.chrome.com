---
layout: 'layouts/doc-post.njk'
title: Create the service worker
subhead: ''
description: ''
date: 2022-11-15
---

[INTRO TBD]

Service workers replace background scripts in Manifest V3 to ensure that background code stays off the main thread where it can hurt extension performance. 

## Differences between background scripts and extension service workers {: #differences }

Many still call extension service workers 'background scripts'. Although extension service workers do run in the background, calling them background scripts is somewhat misleading by implying identical capabilities. The differences are described below.

| Background pages                         | Extension service worker                          |
|------------------------------------------|---------------------------------------------------|
| Persists when not in use.                | Terminates when not in use.                       |
| Can access the DOM and `window`.         | Cannot access to the DOM or `window`.             |
| Can use `XMLHttpRequest()`.              | Must use [`fetch()`][mdn-fetch] to make requests. |
| Can block other pages in the extension.  | Will never block pages in the extension.          |

## Updating the manifest.json {: updating-the-manifest }

In addition to the tasks below, you will also need to update the `"background"` element in your `manifest.json` file. For specifics, see [the section in Update the manifest](/docs/extensions/migrate-to-mv3/upgrade-the-manifest#man-sw).

## Move event listeners to the top level {: #move-event-listeners }

[INTRO TBD]

The following shows a way that events could be registered in Manifest V2. This is **not** guaranteed to work in Manivest V3 because the extension [Storage APIs](/docs/extensions/reference/storage/) are asynchronous.

```js
chrome.storage.local.get(["badgeText"], ({ badgeText }) => {
  chrome.action.setBadgeText({ text: badgeText });
  chrome.action.onClicked.addListener(handleActionClick);
});
``` 

[SOMETHING ABOUT STARTUP ORDER - TBD]
[SECOND HALF OF NEXT SENTENCE MAY NOT BE NEEDED]

Instead, move the event listener registration to the top level of your script. This ensures that Chrome will be able to immediately find and invoke your action's click handler, even if your extension hasn't finished executing its async startup logic.

```js
chrome.action.onClicked.addListener(handleActionClick);

chrome.storage.local.get(["badgeText"], ({ badgeText }) => {
  chrome.action.setBadgeText({ text: badgeText });
});
```

## Saving data across service worker restarts

[TBD - Information about using the new in-memory StorageArea]