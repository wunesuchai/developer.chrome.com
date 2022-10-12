---
layout: 'layouts/doc-post.njk'
title: Building network requests
subhead: ''
description: ''
date: 2022-11-09
---

Extensions that modify network requests will need to transition from the blocking version of the
[Web Request API][api-webrequest] to the new [Declarative Net Request
API][api-declarativenetrequest]. This new API was designed to work well with the event-based
execution model of service workers and to maximize an extension's ability to block network requests
without requiring the extension to have permissions.

## Can Manifest V3 extensions use blocking Web Request?  {: #when-use-blocking-webrequest }

The blocking version of the [Web Request API][api-webrequest] exists in Manifest V3, but it can only be used by extensions that are force-installed using Chrome's enterprise policies: [ExtensionSettings][enterprise-settings], [ExtensionInstallForcelist][enterprise-force-list].

Extensions for use by the general public&mdash;meaning those published on the Chrome Web Store&mdash; must now use [Declarative Net Request][api-declarativenetrequest] for network request modification. The exceptions to this requirment are for extensions deployed to a given domain or to trusted testers. 

## How do you use declarativeNetRequest?  {: #how-use-declarativenetrequest }

Instead of reading the request and programmatically altering it, your extension specifies a number of [rules][api-declarative-rules]. Each rule contains a set of actions to perform when a given set of conditions are matched. For example, you could define a rule that removes "cookie" headers when a request is sent to a specific domain. See the [`"declarativeNetRequest"`][api-declarativenetrequest] reference documentation for a more detailed description of rules.

This feature allows content blockers and other request-modifying extensions to implement their use cases without requiring host permissions, and without needing to read actual requests.

## Conditional permissions and declarativeNetRequest  {: #declarativenetrequest-conditional-perms }

Most use cases for `"declarativeNetRequest"` don't require any host permissions. However, some
do.

{% Aside 'caution' %}
Host permissions are still required if the extension wants to *redirect* a request or modify its headers. The `declarativeNetRequestWithHostAccess` permission always requires host permissions to the request URL and its initiator to act on a request.
{% endAside %}

When extensions require host permissions, we recommend a "tiered" permissions strategy. This means implementing the extension's core functionality without requiring permissions, then putting the more advanced use cases in [the `"optional_host_permissions"`][optional-host-permissions] field.

This approach allows privacy-conscious users to withhold those permissions and still use much of the extension's functionality. This means that developers can implement many common use cases, such as content-blocking functionality, without requiring any host permissions.