---
layout: 'layouts/doc-post.njk'
title: Migrating to Manifest V3
subhead: 'Getting you headed in the right direction.'
description: 'A high-level guide to how you can migrate your Manifest V2 extensions to Manifest V3.'
date: 2020-11-09
updated: 2022-06-13
---

This guide provides developers with the information they need to begin migrating an extension from
Manifest V2 to Manifest V3. Some extensions will require very little change to make them Manifest V3
compliant, while others will need to be redesigned to some degree. For a quick reference guide see
the [migration checklist][mv3-checklist].

{% Aside %}

Follow [What's new in Chrome Extensions][doc-whats-new] to read about new Manifest V3 features as
they become available.

{% endAside %}

### Update host permissions in your manifest {: #host-permissions }

In Manifest V3, you need to specify host permissions and optional host permissions separately
from other permissions. 

{% Columns %}
```js
// Manifest V2
{
  ...
  "permissions": [
    "tabs",
    "bookmarks",
    "http://www.blogger.com/",
  ],
  "optional_permissions": [
    "unlimitedStorage",
    "*://*/*",
  ]
  ...
}
```

```js/10-15
// Manifest V3
{
  ...
  "permissions": [
    "tabs",
    "bookmarks"
  ],
  "optional_permissions": [
    "unlimitedStorage"
  ],
  "host_permissions": [
    "http://www.blogger.com/",
  ],
  "optional_host_permissions": [
    "*://*/*",
  ]
  ...
}
```
{% endColumns %}

{% Aside 'caution' %}

Moving the match patterns to `"host_permissions"` does not affect [content scripts][doc-static-cs]. Content script match patterns remain under `"content_scripts.matches"`. See [Inject with static declarations][static-declarative] for information on `"content_scripts.matches"`.

{% endAside %}


### Convert your background pages to a service worker {: #man-sw }

In Manifest V3, background pages are now *service workers*. Register the service worker under the `"background"` field. This field uses the `"service_worker"` key, which specifies a single JavaScript file. Although extension service workers are conceptually the same as web [service workers][mdn-service-workers], there are differences. For more information, see [Migrating from background pages to service workers][doc-background-to-worker]

{% Columns %}

```json
// Manifest V2
{
  ...
  "background": {
    "scripts": [
      "backgroundContextMenus.js",
      "backgroundOauth.js"
    ],
    "persistent": false
  },
  ...
}
```

```json
// Manifest V3
{
  ...
  "background": {
    "service_worker": "background.js",
    "type": "module" //optional
  }
  ...
}
```

{% endColumns %}

Even though Manifest V3, does not support multiple background scripts, you can optionally declare
the service worker as an [ES Module][webdev-esm] by specifying `"type": "module"`, which allows you
to import further code.


### Consolidate the browser_action and page_action properties in the manifest.json {: #action-api-unification }

In Manifest V2, there were two manifest fields for implementing actions: `"browser_action"` and `"page_action"`. These fields filled distinct roles when they were introduced, but over time they've become redundant. In Manifest V3 consolidate them into the `"action"` field:

{% Columns %}
```json
// Manifest V2

// manifest.json
{
  ...
  "browser_action": { ... },
  "page_action": { ... }
  ...
}
```

```json
// Manifest V3

// manifest.json
{
  ...
  "action": { ... }
  ...
}
```
{% endColumns %}


### Consolidate the chrome.browserAction or chrome.pageAction callbacks {: #action-callback-unification }

The action callbacks are also consolidated. When converting your back ground page to a service worker, replace `chrome.browserAction` and `chrome.pageAction` with `chrome.action`.

{% Columns %}
```js
// Manifest V2

// background.js
chrome.browserAction.onClicked.addListener(tab => { ... });
chrome.pageAction.onClicked.addListener(tab => { ... });
```

```js
// Manifest V3

// background.js (the extension service worker)
chrome.action.onClicked.addListener(tab => { ... });
```
{% endColumns %}

### Replace the blocking version of chrome.webRequest with "chrome.declarativeNetRequest"  {: #modifying-network-requests }

Extensions that modify network requests will need to transition from the blocking version of the
[Web Request API][api-webrequest] to the new [Declarative Net Request
API][api-declarativenetrequest]. This new API was designed to work well with the event-based
execution model of service workers and to maximize an extension's ability to block network requests
without requiring the extension to have permissions.

#### Can Manifest V3 extensions use blocking Web Request?  {: #when-use-blocking-webrequest }

The blocking version of the [Web Request API][api-webrequest] exists in Manifest V3, but it can only be used by extensions that are force-installed using Chrome's enterprise policies: [ExtensionSettings][enterprise-settings], [ExtensionInstallForcelist][enterprise-force-list].

Extensions for use by the general public&mdash;meaning those published on the Chrome Web Store&mdash; must now use [Declarative Net Request][api-declarativenetrequest] for network request modification. The exceptions to this requirment are for extensions deployed to a given domain or to trusted testers. 

#### How do you use declarativeNetRequest?  {: #how-use-declarativenetrequest }

Instead of reading the request and programmatically altering it, your extension specifies a number of [rules][api-declarative-rules]. Each rule contains a set of actions to perform when a given set of conditions are matched. For example, you could define a rule that removes "cookie" headers when a request is sent to a specific domain. See the [`"declarativeNetRequest"`][api-declarativenetrequest] reference documentation for a more detailed description of rules.

This feature allows content blockers and other request-modifying extensions to implement their use cases without requiring host permissions, and without needing to read actual requests.

#### Conditional permissions and declarativeNetRequest  {: #declarativenetrequest-conditional-perms }

Most use cases for `"declarativeNetRequest"` don't require any host permissions. However, some
do.

{% Aside 'caution' %}
Host permissions are still required if the extension wants to *redirect* a request or modify its headers. The `declarativeNetRequestWithHostAccess` permission always requires host permissions to the request URL and its initiator to act on a request.
{% endAside %}

When extensions require host permissions, we recommend a "tiered" permissions strategy. This means implementing the extension's core functionality without requiring permissions, then putting the more advanced use cases in [the `"optional_host_permissions"`][optional-host-permissions] field.

This approach allows privacy-conscious users to withhold those permissions and still use much of the extension's functionality. This means that developers can implement many common use cases, such as content-blocking functionality, without requiring any host permissions.

### Use scripting/CSS methods in the Scripting interface {: #api-tabs}

*In Manifest V3, several methods move from the [Tabs interface][api-tabs] to the [Scripting interface][api-scripting].*

- Replace the `"tabs"` permission with the `"scripting"` permission in the `manifest.json` file.
- Remove the `"host_permissions"` and `"activeTab"` permissions from the `manifest.json` file.
- Move `executeScript()`, `insertCSS()`, and `removeCSS()` from the `tabs` interface to the `scripting` interface and do the follwing for all three:
  - Replace the `InjectDetails` object with `ScriptInjection` object.
  - Move the `tabID` from the method arguments to `scriptInjection.target`.

The example shows how do do this for `insertCSS()`. The procedure for `executeScript()` and `removeCSS()` is the same.

{% Columns %}
```js
chrome.tabs.insertScript(tabId, injectDetails, (executionResults) => {
  // callback function
});
```

```js
chrome.scripting.insertCSS(
  {
  files: ["style.css"],
  target: { tabId: tab.id },
  }, (executionResults) => {
    // callback function
  }
);
```
{% endColumns %}


### Remove execution of remote code and arbitrary strings {: #api-remote-code}

*You can no longer [execute external logic][mv3-remote-code] using `executeScript()`, `eval()`, and `new Function()`.*

As stated above, `executeScript()` has moved from the `tabs` interface to the `scripting` interface and has a slightly different interface. Additionally:

- All external code (JS, Wasm, CSS) must be part of your extension bundle.
- Resources used by these files must also be part of your extension bundle.
- Use [`chrome.runtime.getURL()`][runtime-geturl] to build resource URLs at runtime.

### Replace functions that expect a Manifest V2 background context {: #api-background-context}

*Extension scripts, `content.js` and `popup.js` specifically, cannot interact with [extension service workers][mv3-sw], the replacement for background scripts in Manifest V3.*

- Locate calls that expect a bacground context: `chrome.runtime.getBackgroundPage()`, `chrome.extension.getBackgroundPage()`, `chrome.extension.getExtensionTabs()`, and `chrome.extension.getViews()`, specifically.
- Replace them with messages passed via `sendMessage()` from extension scripts.
- Intercept them in the extension service worker using the `onMessage` event handler.

For more information, see [Message passing][doc-messages].

### Remove CORS requests from content scripts {: #security-cors }

- Move these requests to the background service worker.
- Call [`fetch()`][mdn-fetch] rather than `XMLHttpRequest()`.

```js
fetch('https://www.example.com/data.json')
.then(() => {
  console.log('it worked!');
})
.catch(error => {
  console.error(error);
});
```

### Use a custom content_security_policy in manifest.json? {: #content-security-policy }

An extension's [content security policy][csp] (CSP) was specified in Manifest V2 as a string; inManifest V3 it is an object with members representing alternative CSP contexts.

- Replace the value of `"content_security_policy"` with subfields named `"extension_pages"` and `"sandbox"` as appropriate.
- Remove references to external domains in `script-src`, `worker-src`, `object-src`, and
  `style-src` directives if present. 


{% Columns %}
```json
// Manifest V2
{
  ...
  "content_security_policy": "default-src 'self'"
  ...
}
```

```json
// Manifest V3
{
  ...
  "content_security_policy": {
    "extension_pages": "default-src 'self'",
    "sandbox": "..."
  }
  ...
}
```
{% endColumns %}

**`extension_pages`**:  This policy covers pages in your extension, including html files and service
workers.

{% Aside %}

These page types are served from the `chrome-extension://` protocol. For instance, a page in your
extension is `chrome-extension://EXTENSION_ID/foo.html`.

{% endAside %}

**`sandbox`**: This policy covers any [sandboxed extension pages][manifest-sandbox] that your
extension uses.

In addition, Manifest V3 disallows certain CSP modifications for `extension_pages` that were
permitted in Manifest V2. The `script-src,` `object-src`, and `worker-src` directives may only have
the following values:

*   `self`
*   `none`
*   Any localhost source, (`http://localhost`,  `http://127.0.0.1`, or any port on those domains)

CSP modifications for `sandbox` have no such new restrictions.

Starting in Chrome 102, Manifest V3 extensions can include `wasm-unsafe-eval` in the CSP to use WebAssembly
files bundled as part of the extension.

 {: #content-security-policy }

An extension's [content security policy][csp] (CSP) was specified in Manifest V2 as a string; in
Manifest V3 it is an object with members representing alternative CSP contexts:

{% Columns %}
```json
// Manifest V2
{
  ...
  "content_security_policy": "..."
  ...
}
```

```json
// Manifest V3
{
  ...
  "content_security_policy": {
    "extension_pages": "...",
    "sandbox": "..."
  }
  ...
}
```
{% endColumns %}

**`extension_pages`**:  This policy covers pages in your extension, including html files and service
workers.

{% Aside %}

These page types are served from the `chrome-extension://` protocol. For instance, a page in your
extension is `chrome-extension://EXTENSION_ID/foo.html`.

{% endAside %}

**`sandbox`**: This policy covers any [sandboxed extension pages][manifest-sandbox] that your
extension uses.

In addition, Manifest V3 disallows certain CSP modifications for `extension_pages` that were
permitted in Manifest V2. The `script-src,` `object-src`, and `worker-src` directives may only have
the following values:

*   `self`
*   `none`
*   Any localhost source, (`http://localhost`,  `http://127.0.0.1`, or any port on those domains)

CSP modifications for `sandbox` have no such new restrictions.

Starting in Chrome 102, Manifest V3 extensions can include `wasm-unsafe-eval` in the CSP to use WebAssembly
files bundled as part of the extension.



[api-declarative-rules]: /docs/extensions/reference/declarativeNetRequest/#example
[api-declarativenetrequest]: /docs/extensions/reference/declarativeNetRequest
[api-scripting-executescript]: /docs/extensions/reference/scripting/#method-executeScript
[api-scripting]: /docs/extensions/reference/scripting
[api-tabs]: /docs/extensions/reference/tabs
[api-tabs-example]: /docs/extensions/reference/tabs/#get-the-current-tab
[api-tabs-executescript]: /docs/extensions/reference/tabs/#method-executeScript 
[api-webrequest]: /docs/extensions/reference/webRequest
[bootstrap-gs]: https://getbootstrap.com/docs/5.1/getting-started/introduction/
[csp]: https://content-security-policy.com/
[dev-google-sw]: https://developers.google.com/web/fundamentals/primers/service-workers
[doc-background-to-worker]: /docs/extensions/mv3/migrating_to_service_workers
[doc-manifest]: /docs/extensions/mv3/manifest
[doc-match-pattern]: /docs/extensions/mv3/match_patterns/
[doc-perm-warn]: /docs/extensions/mv3/permission_warnings/
[doc-static-cs]: /docs/extensions/mv3/content_scripts/#static-declarative
[doc-war]: /docs/extensions/mv3/manifest/web_accessible_resources/
[doc-whats-new]: /docs/extensions/whatsnew/
[enterprise-force-list]: https://cloud.google.com/docs/chrome-enterprise/policies/?policy=ExtensionInstallForcelist
[enterprise-settings]: https://cloud.google.com/docs/chrome-enterprise/policies/?policy=ExtensionSettings
[github-samples-content]: https://github.com/GoogleChrome/chrome-extensions-samples/tree/main/reference/mv3/intro/mv3-migration/content-scripts
[manifest-sandbox]: /docs/extensions/mv3/manifest/sandbox
[mdn-cdn]: https://developer.mozilla.org/docs/Glossary/CDN
[mdn-eval]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/eval
[mdn-fetch]: https://developer.mozilla.org/docs/Web/API/Fetch_API/Using_Fetch
[mdn-service-workers]: https://developer.mozilla.org/docs/Web/API/Service_Worker_API
[mv3-checklist]: /docs/extensions/mv3/mv3-migration-checklist
[mv3-declarative]: /docs/extensions/reference/declarativeNetRequest
[mv3-network]: /docs/extensions/mv3/intro/mv3-overview#network-request-modification
[mv3-other]: /docs/extensions/mv3/intro/mv3-overview#other-features
[mv3-overview]: /docs/extensions/mv3/intro/mv3-overview
[mv3-promise]: /docs/extensions/mv3/intro/mv3-overview#promises
[mv3-remote]: /docs/extensions/mv3/intro/mv3-overview#remotely-hosted-code
[mv3-sw]: /docs/extensions/mv3/intro/mv3-overview#service-workers
[optional-host-permissions]: /docs/extensions/reference/permissions/#step-2-declare-optional-permissions-in-the-manifest
[react-cdn]: https://reactjs.org/docs/cdn-links.html
[section-action]: #action-api-unification
[section-csp]: #content-security-policy
[section-file]: #inject-file
[section-func]: #inject-func
[section-host]: #host-permissions
[section-man-sw]: #man-sw
[section-war]: #web-accessible-resources
[static-declarative]: /docs/extensions/mv3/content_scripts/#static-declarative
[webdev-esm]: https://web.dev/es-modules-in-sw/#static-imports-only