---
layout: 'layouts/doc-post.njk'
title: Update the manifest
subhead: 'Convert a V2 manifest to a V3 manifest'
description: 'TBD'
date: 2022-11-15
---

The `manifest.json` requires a slightly different format for V3 than for V2. Some of these changes may be made on their own. Others require parallel changes to your code. 

In addition to the changes listed here, the following upgrades require additional manifest changes, which will be described with the features they support.

* Service worker related changes
* Improvements to content security policy
* New scripting permissions.

## Updates that affect only the manifest.json

### Change the version number  {: #manifest-version }

Change the value of the `"manifest_version"` element from 2 to 3.

<div class="switcher">
{% Compare 'worse' 'Manifest V2' %}
```json
{
  ...
  "manifest_version": 2
  ...
}
```

{% endCompare %}

{% Compare 'better' 'Manifest V3' %}
```json
{
  ...
  "manifest_version": 3
  ...
}
```

{% endCompare %}
</div>

### Update host permissions {: #host-permissions }

Host permissions in Manifest V3 are a separate element; you don't specify them in `"permissions"` or `"optional_permissions"`. 

<div class="switcher">
{% Compare 'worse' 'Manifest V2' %}
```json
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

{% endCompare %}

{% Compare 'better' 'Manifest V3' %}
```jsonjs/9-14
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

{% endCompare %}
</div>

{% Aside 'caution' %}
Do not move [content scripts](/docs/extensions/mv3/content_scripts/#static-declarative) to `"host_permissions"`. Content script match patterns remain under `"content_scripts.matches"`. See [Inject with static declarations](/docs/extensions/mv3/content_scripts/#static-declarative) for information on `"content_scripts.matches"`.
{% endAside %}


<!-- Seems like this might require code changes -->
### Update web accessible resources {: #web-accessible-resources }

As implemented in Manifest V2, the `"web_accessible_resources"` made extension detectable by websites and attackers. This created opportunities for fingerprinting or unintentional resource access. Manifest V3 limits exposure by restricting which web sites and extensions can access resources in your extension. Instead of providing a list of files as before, you now provide an **array of objects**, each of which maps a set of resources to a set of URLs or extension IDs. For more information, see [Web Accessible rResources](/docs/extensions/mv3/manifest/web_accessible_resources/).

<div class="switcher">
{% Compare 'worse' 'Manifest V2' %}
```json
{
  ...
  "web_accessible_resources": [
    "images/*.png",
    "style/extension.css",
    "script/extension.js"
  ],
  ...
}
```

{% endCompare %}

{% Compare 'better' 'Manifest V3' %}
```json/3-6
{
  ...
  "web_accessible_resources": [
    {
      "resources": [ "images/*.png", "style/extension.css", "script/extension.js" ],
      "matches": [ "https://example.com/*" ]
    }, 
    {
      "resources": [ "test1.png", "test2.png" ],
      "matches": [ "https://someotherwebsite.com/*" ],
      "use_dynamic_url": true
    }
  ],
  ...
}
```

{% endCompare %}
</div>

## Manifest updates that also require code changes

[INTRO TBD]

### Convert background pages to a service worker {: #man-sw }

In Manifest V3, background pages are now *service workers*. Although extension service workers are conceptually the same as web [service workers](https://developer.mozilla.org/docs/Web/API/Service_Worker_API), there are differences. The manifest changes are listed below. For instructions on upgrading your scripts, see [Migrating from background pages to service workers](/docs/extensions/mv3/migrating_to_service_workers).

- Replace `"background.page"` or `"background.scripts"` with `"background.service_worker"` in the `manifest.json`. Note that the `"service_worker"` field takes a string, not an array of strings.
- Remove `"background.persistent"` from the `manifest.json`.
- [Update background scripts](/docs/extensions/mv3/migrating_to_service_workers) to adapt to the service worker execution context.

Even though Manifest V3 does not support multiple background scripts, you can optionally declare the service worker as an [ES Module](https://web.dev/es-modules-in-sw/#static-imports-only) by specifying `"type": "module"`, which allows you to import further code.

<div class="switcher">
{% Compare 'worse' 'Manifest V2' %}
```json
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

{% endCompare %}

{% Compare 'better' 'Manifest V3' %}
```json
{
  ...
  "background": {
    "service_worker": "background.js",
    "type": "module" //optional
  }
  ...
}
```

{% endCompare %}
</div>


### Consolidate the "browser_action" and "page_action" fields {: #action-api-unification }

In Manifest V2, there were two manifest fields for implementing actions: `"browser_action"` and `"page_action"`. These fields filled distinct roles when they were introduced, but over time they've become redundant. In Manifest V3 consolidate them into the `"action"` field.

<!-- This previously included an example of what's in the background scripts. Should that still be here? -->

<div class="switcher">
{% Compare 'worse' 'Manifest V2' %}
```json
{
  ...
  "browser_action": { ... },
  "page_action": { ... }
  ...
}
```

{% endCompare %}

{% Compare 'better' 'Manifest V3' %}
```json
{
  ...
  "action": { ... }
  ...
}
```

{% endCompare %}
</div>

### Consolidate the chrome.browserAction or chrome.pageAction callbacks {: #action-callback-unification }

The action callbacks are also consolidated. When converting your back ground page to a service worker, replace `chrome.browserAction` and `chrome.pageAction` with `chrome.action`.

<div class="switcher">
{% Compare 'worse' 'A background.js' %}
```js
chrome.browserAction.onClicked.addListener(tab => { ... });
chrome.pageAction.onClicked.addListener(tab => { ... });
```

{% endCompare %}

{% Compare 'better' 'The extension service worker' %}
```js
chrome.action.onClicked.addListener(tab => { ... });
```

{% endCompare %}
</div>

### Replace callbacks on the chrome.webRequest interface with "declarative_net_request" fields {: #modifying-network-requests }

Extensions that modify network requests need to replace callbacks on the `chrome.webRequest` interface with declarations in the `manifest.json` file. 

{% Aside %}
This only applies to user-installed extensions (those installed from the Chrome Web Store). Force installed extensions (extensions distributed using `ExtensionInstallForcelist` and typically used in an enterprise setting) can still use the blocking version of `chrome.webRequest`, though they are deprecated.
{% endAside %}

- Add the `"declarative_net_request"` field and its child `"rule_resources"` to your `manifest.json`. `"rule_resources"` is an array. For more information on `"rule_resources"`, see [Building network requests](/docs/extensions/mv3/building-network-requests/).
- Replace the `"webRequestBlocking"` [permission](/docs/extensions/reference/permissions/) with the `"declarativeNetRequest"` permission.
- Remove the `webRequest` permission if you no longer need to observe network requests.
- Remove unnecessary host permissions; blocking a request or upgrading a request's protocol
  doesn't require host permissions with `"declarativeNetRequest"`.

```json
"declarative_net_request": {
  "rule_resources": [{
    "id": "ruleset_1",
    "enabled": true,
    "path": "rules.json"
  }]
},
"permissions": [ "declarativeNetRequest" ],
```

