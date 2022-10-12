---
layout: 'layouts/doc-post.njk'
title: "Manifest V3 migration checklist"
date: 2019-11-02
updated: 2022-11-15
description: A quick reference on migrating your Chrome Extensions from Manifest V2 to Manifest V3.
---

Welcome to Manifest V3. The checklists below is here to help you keep track of your upgrade work. It defines tasks that must be completed with links to instructions for the work.

{% Details  'open' %}

{% DetailsSummary %}
## Update manifest.json {: update-manifest }
{% endDetailsSummary %}

The `manifest.json` requires a slightly different format for V3 than for V2. Some of these changes may be made on their own. Others require parallel changes to your code.

### Updates that affect only the manifest.json

Perform the tasks below to convert your `manifest.json` file from the Manifest V2 format to the Manifest V3 format.

* [Change the version number](/docs/extensions/migrate-to-mv3/update-the-manifest/#manifest-version). {: change-version }
* [Update host permissions](/docs/extensions/migrate-to-mv3/update-the-manifest/#host-permissions). {: #api-host-perms}
* [Update web accessible resources](/docs/extensions/migrate-to-mv3/update-the-manifest/#web-accessible-resources).


### Manifest updates that also require code changes

The following tasks require changes to both your `manifest.json` and to code.

* [Convert background pages to a service worker](/docs/extensions/migrate-to-mv3/update-the-manifest/#man-sw). {: #api-background-pages }
* [Consolidate the `"browser_action"` and `"page_action"` fields](/docs/extensions/migrate-to-mv3/update-the-manifest/#action-api-unification). {: #api-browser-action-manifest}]
* [Consolidate the chrome.browserAction or chrome.pageAction callbacks](/docs/extensions/migrate-to-mv3/update-the-manifest/#action-callback-unification).
* [Replace callbacks on the `chrome.webRequest` interface with `"chrome.declarativeNetRequest"` fields](/docs/extensions/migrate-to-mv3/update-the-manifest/#modifying-network-requests).

{% endDetails %}


{% endDetails %}

{% Details  'open' %}

{% DetailsSummary %}
## Upgrade to a service worker
{% endDetailsSummary %}

A single extension service workers replace background scripts to ensure that background code stays off the main thread where it can hurt performance.

* [Move event listeners to the top level](/docs/extensions/migrate-to-mv3/upgrade to-a-service-worker#move-event-listeners)

{% endDetails %}


{% Details  'open' %}

{% DetailsSummary %}
## Update your code
{% endDetailsSummary %}

Some features need to be replaced with more modern equivalents. Other need to be removed entirely.

* [Replace `tabs.executeScript()` with `scripting.executeScript()`](/docs/extensions/migrate-to-mv3/update-code#move-executescript).
* [Move `insertCSS()` and `removeCSS(`) from the `tabs` interface to the `scripting` interface](/docs/extensions/migrate-to-mv3/update-code#move-css-calls).
* [Replace functions that expect a Manifest V2 background context](/docs/extensions/migrate-to-mv3/update-code#api-background-context).
* [Replace calls to `XMLHttpRequest()` with global `fetch()`](/docs/extensions/migrate-to-mv3/update-code#use-global-fetch).
* [Remove calls to unsupported code](/docs/extensions/migrate-to-mv3/update-code#remove-unsupported)

Additionally:

* [Replace blocking web requests](/docs/extensions/migrate-to-mv3/migrate-web-requests).

{% endDetails %}

{% Details  'open' %}

{% DetailsSummary %}
## Improve extension security
{% endDetailsSummary %}

* [Remove execution of arbitrary strings](/docs/extensions/migrate-to-mv3/improve-extension-security#arbitrary-strings).
* [Eliminate remotely hosted code](/docs/extensions/migrate-to-mv3/improve-extension-security#remotely-hosted-code)
* [Update content security policy in manifest.json](/docs/extensions/migrate-to-mv3/improve-extension-security#security-csp).
* [Remove unsupported content security policy rules](/docs/extensions/migrate-to-mv3/improve-extension-security#remove-unsupported-csp).

{% endDetails %}
