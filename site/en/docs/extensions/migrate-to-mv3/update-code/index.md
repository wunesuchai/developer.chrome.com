---
layout: 'layouts/doc-post.njk'
title: Refactor your code
subhead: ''
description: ''
date: 2022-11-15
---

[INTRO TBD]

## Replace tabs.executeScript() with scripting.executeScript() {: #move-executescript }

<!-- The content in this section is from two sources. They need to be reconciled. -->


[INTRO TBD]

Before making any of the changes in this section do the following in your `manifest.json` file:
* Replace the `"tabs"` permission with the `"scripting"` permission.
* Remove the `"host_permissions"` and `"activeTab"` permissions.

### Injuecting a static file {: #cs-static-file}

Static file injection with `scripting.executeScript()` is similar to how it worked with `tabs.executeScript()`. While the old method only took a single file, the new method now takes an array of files. Note that the `tabId` has been moved from arguments to a member of the injected object, which now uses [`ScriptInjection`](/docs/extensions/reference/scripting/#type-ScriptInjection) instead of `InjectDetails`. 

<div class="switcher">
{% Compare 'worse' 'Manifest V2' %}
```json
chrome.tabs.executeScript(
  tab.id,
  {
  file: 'content-script.js'
  }
);
```

{% CompareCaption %}
In a background script file.
{% endCompareCaption %}

{% endCompare %}

{% Compare 'better' 'Manifest V3' %}
```json
async function getCurrentTab() {/* ... */}
let tab = await getCurrentTab();

chrome.scripting.executeScript({
  target: {tabId: tab.id},
  files: ['content-script.js']
});
```

{% CompareCaption %}
In the background service worker.
{% endCompareCaption %}

{% endCompare %}
</div>

### Injecting an external library

External libraries may no longer be loaded remotely. It must be part of your extension bundle. Load it at run time by adding it to the files array.

```js
chrome.scripting.executeScript({
  target: {tabId: tab.id},
  files: ['jquery-min.js', 'content-script.js']
});
```

#### Injecting a function {: #cs-func } 

If you need more dynamism, the new `func` property allows you to inject a function as a content script and pass variables using the `args` property. Note that the function is not run as if it were located within the content script; rather, its source is sent to the target tab and it is run there.

<div class="switcher">
{% Compare 'worse' 'Manifest V2' %}
```json
let name = 'World!';
chrome.tabs.executeScript({
  code: `alert('Hello, ${name}!')`
});
```

{% CompareCaption %}
In a background script file.
{% endCompareCaption %}

{% endCompare %}

{% Compare 'better' 'Manifest V3' %}
```json
async function getCurrentTab() {/* ... */}
let tab = await getCurrentTab();

function showAlert(givenName) {
  alert(`Hello, ${givenName}`);
}

let name = 'World';
chrome.scripting.executeScript({
  target: {tabId: tab.id},
  func: showAlert,
  args: [name],
});
```

{% CompareCaption %}
In the background service worker.
{% endCompareCaption %}

{% endCompare %}
</div>

You can find functional versions of the examples in this section in the [chrome-extensions-samples](https://github.com/GoogleChrome/chrome-extensions-samples/tree/main/reference/mv3/intro/) repository. See the [Tabs API
examples](/docs/extensions/reference/tabs/#get-the-current-tab) for an implementation of `getCurrentTab()`.

## Move insertCSS() and removeCSS() from the tabs interface to the scripting interface {: #move-css-calls }


  - Replace the `InjectDetails` object with `ScriptInjection` object.
  - Move the `tabID` from the method arguments to `scriptInjection.target`.

The example shows how do do this for `insertCSS()`. The procedure for `executeScript()` and `removeCSS()` is the same.

<div class="switcher">
{% Compare 'worse' 'Manifest V2' %}
```js
chrome.tabs.insertScript(tabId, injectDetails, (executionResults) => {
  // callback function
});
```

{% CompareCaption %}
In a background script file.
{% endCompareCaption %}

{% endCompare %}

{% Compare 'better' 'Manifest V3' %}
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

{% CompareCaption %}
In the background service worker.
{% endCompareCaption %}

{% endCompare %}
</div>

If you have not already done so, do the following in your `manifest.json` file:
* Replace the `"tabs"` permission with the `"scripting"` permission.
* Remove the `"host_permissions"` and `"activeTab"` permissions.

## Replace functions that expect a Manifest V2 background context {: #api-background-context}

Extension scripts, `content.js` and `popup.js` specifically, cannot interact with [extension service workers][mv3-sw], the replacement for background scripts in Manifest V3.

- Locate calls that expect a bacground context: `chrome.runtime.getBackgroundPage()`, `chrome.extension.getBackgroundPage()`, `chrome.extension.getExtensionTabs()`, and `chrome.extension.getViews()`, specifically.
- Replace them with messages passed via `sendMessage()` from extension scripts.
- Intercept them in the extension service worker using the `onMessage` event handler.

For more information, see [Message passing](/docs/extensions/mv3/messaging/).

## Replace calls to XMLHttpRequest() with global fetch() {: #use-global-fetch }

There are actually three isues here. Since before Manifest V2, it's been true that CORS requests may or many not work on content scripts since these scripts must conform to the CORS requirements of whatever page they control. This requires that requests for external resources be moved to the extension service workers. 

Since `XMLHttpRequest()` cannot be called from a service worker, this also means that such calls need to be reimplemented as calls to [global `fetch()`](https://developer.mozilla.org/en-US/docs/Web/API/fetch). 

```js
fetch('https://www.example.com/data.json')
.then(() => {
  console.log('it worked!');
})
.catch(error => {
  console.error(error);
});
```

Finally, and as stated elsewhere, remote code can no longer be executed in an extension. All executable code must be part of your extension package.

## Remove unsupported APIs {: #remove-unsupported}

The methods and properties listed below are not supported in Manifest V3. 

*   chrome.extension.connect()
*   chrome.extension.getExtensionTabs()
*   chrome.extension.getURL()
*   chrome.extension.lastError
*   chrome.extension.onConnect
*   chrome.extension.onMessage
*   chrome.extension.onRequest
*   chrome.extension.onRequestExternal
*   chrome.extension.sendRequest()
*   chrome.tabs.getAllInWindow()
*   chrome.tabs.getSelected()
*   chrome.tabs.onActiveChanged
*   chrome.tabs.onHighlightChanged
*   chrome.tabs.onSelectionChanged
*   chrome.extension.sendMessage()
*   chrome.tabs.sendRequest()
*   chrome.tabs.Tab.selected

