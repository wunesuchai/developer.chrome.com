---
layout: 'layouts/doc-post.njk'
title: "Manifest V3 migration checklist"
date: 2019-11-02
updated: 2022-10-08
description: A quick reference on migrating your Chrome Extensions from Manifest V2 to Manifest V3.

---

This page provides a quick reference to help you identify any changes you might need to
make to an Manifest V2 extension so that it works under Manifest V3. For more
description of the nature of these changes see the [Manifest V3 migration guide][mv3-migration-guide].

## API checklist {: #api_checklist }

There are some changes you may need to make based on changes to the API surface. This section lists these changes.

### Update host permissions in your manifest {: #api-host-perms}

*Host permissions in Manifest V3 are a separate element; you don't specify them in `permissions` or `optional_permissions`.*

- [Move host permissions][mv3-host-perms] into the `host_permissions` field in the manifest.json.


### Convert your background pages to a service worker {: #api-background-pages }

*Manifest V3 [uses service workers instead of background pages.*

- Replace `background.page` or `background.scripts` with `background.service_worker` in the `manifest.json`. Note that the `service_worker` field takes a string, not an array of strings.
- Remove `background.persistent` from the `manifest.json`.
- [Update background scripts][background-to-sw] to adapt to the service worker execution context.

### Consolidate the "browser_action" and "page_action" fields in the manifest.json {: #api-browser-action-manifest}

*These fields are consolidated into a single field in Manifest V3.*

- [Replace these fields][mv3-action] with `"action"`.

### Consolidate the chrome.browserAction or chrome.pageAction callbacks {: #api-browser-action-js}

*These two equivalent APIs are [consolidated into a single callback in Manifest V3.*

- [Replace these callbacks][mv3-action] with `chrome.action`.


### Replace the blocking version of chrome.webRequest with "chrome.declarativeNetRequest" {: #api-blocking}

*Use `"chrome.declarativeNetRequest"` to replace the `chrome.webRequest` interface.*

{% Aside %} This only applies to user-installed extensions. Force installed extensions (extensions distributed using ExtensionInstallForcelist and typically used in an enterprise setting) can still use the blocking version of chrome.webRequest. {% endAside %}

- Replace request modification logic with `"chrome.declarativeNetRequest"` rules.
- Replace the `"webRequestBlocking"` [permission][permissions] with `"declarativeNetRequest"`.
- Remove the `webRequest` permission if you no longer need to observe network requests.
- Remove unnecessary host permissions; blocking a request or upgrading a request's protocol
  doesn't require host permissions with `"declarativeNetRequest"`.


### Use scripting/CSS methods in the Scripting interface {: #api-tabs}

*In Manifest V3, several methods move from the [Tabs interface][api-tabs] to the [Scripting interface][api-scripting].*

- Move calls to `executeScript()`, `insertScript()`, and `removeCSS()` from the `tabs` interface to the `scripting` interface. See the [section in][mv3-scripting] *Migrating to Manifest V3* for information on changes to the objects passed to these functions.





[api-action]: /docs/extensions/reference/action
[api-scripting]: /docs/extensions/reference/scripting
[api-tabs]: /docs/extensions/reference/tabs
[background-to-sw]: /docs/extensions/mv3/migrating_to_service_workers/
[chromium-force-install]: https://www.chromium.org/administrators/policy-list-3#ExtensionInstallForcelist
[mv3-action]: /docs/extensions/mv3/intro/mv3-migration#action-api-unification
[mv3-host-perms]: /docs/extensions/mv3/intro/mv3-migration#host-permissions
[mv3-migration-guide]: /docs/extensions/mv3/intro/mv3-migration
[mv3-network-request]: /docs/extensions/mv3/intro/mv3-migration#modifying-network-requests
[mv3-remote-code]: /docs/extensions/mv3/intro/mv3-migration#remotely-hosted-code
[mv3-scripting]: /docs/extensions/mv3/intro/mv3-migration/#api-tabs
[mv3-sw]: /docs/extensions/mv3/intro/mv3-migration#background-service-workers
[permissions]: /docs/extensions/reference/permissions/
[runtime-geturl]: /docs/extensions/reference/runtime/#method-getURL
[doc-messages]: /docs/extensions/mv3/messaging/