











### Are you executing remote code or arbitrary strings? {: #api-remote-code}

*You can no longer [execute external
logic][mv3-remote-code] using `chrome.scripting.executeScript({code: '...'})`, `eval()`, and `new Function()`.*

- Move all external code (JS, Wasm, CSS) into your extension bundle.
- Update script and style references to load resources from the extension bundle.
- Use [`chrome.runtime.getURL()`][runtime-geturl] to build resource URLs at runtime.

### Are you executing functions that expect an Manifest V2 background context? {: #api-background-context}

*The [adoption of service workers][mv3-sw] in Manifest V3 isn't compatible with methods like `chrome.runtime.getBackgroundPage()`,
`chrome.extension.getBackgroundPage()`, `chrome.extension.getExtensionTabs()`,
and `chrome.extension.getViews()`.*

- Migrate to a design that [passes messages][doc-messages] between other contexts and the background service worker.

## Security Checklist {: #security_checklist }

There are some changes you may need to make based on changes in security policy. This section lists these changes.

### Are you making CORS requests in content scripts? {: #security-cors }

- Move these requests to the background service worker.

### Are you using a custom `content_security_policy` in manifest.json? {: #security-csp }

- Replace `content_security_policy` with `content_security_policy.extension_pages`
  or `content_security_policy.sandbox` as appropriate.
- Remove references to external domains in `script-src`, `worker-src`, `object-src`, and
  `style-src` directives if present.

[api-action]: /docs/extensions/reference/action
[api-scripting]: /docs/extensions/reference/scripting
[api-tabs]: /docs/extensions/reference/tabs
[chromium-force-install]: https://www.chromium.org/administrators/policy-list-3#ExtensionInstallForcelist
[mv3-action]: /docs/extensions/mv3/intro/mv3-migration#action-api-unification
[mv3-host-perms]: /docs/extensions/mv3/intro/mv3-migration#host-permissions
[mv3-migration-guide]: /docs/extensions/mv3/intro/mv3-migration
[mv3-network-request]: /docs/extensions/mv3/intro/mv3-migration#modifying-network-requests
[mv3-remote-code]: /docs/extensions/mv3/intro/mv3-migration#remotely-hosted-code
[mv3-sw]: /docs/extensions/mv3/intro/mv3-migration#background-service-workers
[runtime-geturl]: /docs/extensions/reference/runtime/#method-getURL
[doc-messages]: /docs/extensions/mv3/messaging/

