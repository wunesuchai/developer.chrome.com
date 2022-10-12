---
layout: 'layouts/doc-post.njk'
title: Improve extension security
subhead: ''
description: ''
date: 2022-11-15
---

[INTRO TBD]

## Remove execution of arbitrary strings {: #arbitrary-strings }

You can no longer [execute external logic](/docs/extensions/mv3/intro/mv3-overview#remotely-hosted-code) using `executeScript()`, `eval()`, and `new Function()`.

- Move all external code (JS, Wasm, CSS) into your extension bundle.
- Update script and style references to load resources from the extension bundle.
- Use [`chrome.runtime.getURL()`][runtime-geturl] to build resource URLs at runtime.


## Executing arbitrary strings {: #executing-arbitrary-strings }

In Manifest V2, it was possible to execute an arbitrary string of code using
[`tabs.executeScript()`][api-tabs-executescript] and the `code` property on the options object.
Manifest V3 does not allow arbitrary code execution. In order to adapt to this requirement,
extension developers can use the [`scripting.executeScript()`][api-scripting-executescript] method to
either inject a static file or a function.

To use the [Scripting API][api-scripting], you need to include the `"scripting"` permission in your
manifest file. This API does not [trigger a permission warning][doc-perm-warn].

```json
{
  "manifest_version": 3,
  "permissions": ["scripting"],
  ...
}
```




## Eliminate remotely hosted code {: #remotely-hosted-code}

_Remotely hosted code_ is any code that is **not** included in an extension's package as a loadable resource. For example, the following are considered remotely hosted code:

- JavaScript files pulled from the developer's server.
- Any library hosted on a [CDN][mdn-cdn].

In Manifest V3, all of your extension's logic must be included in the extension . You can no longer load and execute a remotely hosted file. A number of alternative approaches are available, depending on your use case and the reason for remote hosting. Here are approaches to consider:

### Configuration-driven features and logic

In this approach, your extension loads a remote configuration (for example a JSON file) at runtime and caches the configuration locally. The extension then uses this cached configuration to decide which features to enable.

### Externalize logic with a remote service

Consider migrating application logic from the extension to a remote web service that your extension can call. (Essentially a form of message passing.) This provides the ability to keep code private and change the code on demand while avoiding the extra overhead of resubmitting to the Chrome Web Store.

### Bundle third-party libraries
If you are using a popular framework such as [React](https://reactjs.org/docs/cdn-links.html) or [Bootstrap](https://getbootstrap.com/docs/5.1/getting-started/introduction/), you can download the minified files, add them to your project and import them locally. The following example shows how do do this in the `popup.html`.

```html
<script src="./react-dom.production.min.js"></script>
<link href="./bootstrap.min.css" rel="stylesheet">
```

To include a library in a service worker, you have two options:
- For standard service workers, use `importScripts()`.
- To use static import statements, set the `"background.type"` to `"module"` in the manifest.

## Update content security policy in manifest.json {: #security-csp }

The `"content_security_policy"` has not been removed from the `manifest.json` file, but it now requires two children: `"extension_pages"` and `"sandbox"`.

<div class="switcher">
{% Compare 'worse' 'Manifest V2' %}
```json
{
  ...
  "content_security_policy": "default-src 'self'"
  ...
}
```


{% endCompare %}

{% Compare 'better' 'Manifest V3' %}
```json
{
  ...
  "content_security_policy": {
    "extension_pages": "default-src 'self'",
    "sandbox": "..."
  }
  ...
}
```


{% endCompare %}
</div>

**`extension_pages`**:  This policy covers pages in your extension, including html files and service workers.

{% Aside %}
These page types are served from the `chrome-extension://` protocol. For instance, a page in your extension is `chrome-extension://EXTENSION_ID/foo.html`.
{% endAside %}

**`sandbox`**: This policy covers any [sandboxed extension pages](/docs/extensions/mv3/manifest/sandbox) that your extension uses.

## Remove unsupported content security policy rules {: #remove-unsupported-csp }

In addition, Manifest V3 disallows content security policy values for `"extension_pages"` that were permitted in Manifest V2. The `script-src,` `object-src`, and `worker-src` directives may only have the following values:

*   `self`
*   `none`
*   Any localhost source, (`http://localhost`,  `http://127.0.0.1`, or any port on those domains)

content security policy values for `sandbox` have no such new restrictions.

Starting in Chrome 102, Manifest V3 extensions can include `wasm-unsafe-eval` in the CSP to use WebAssembly
files bundled as part of the extension.