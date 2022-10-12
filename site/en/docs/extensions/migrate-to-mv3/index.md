---
layout: 'layouts/doc-post.njk'
title: Migrating to Manifest V3
subhead: 'Getting you headed in the right direction.'
description: 'A guide to upgrading Manifest V2 extensions to Manifest V3 extensions.'
date: 2020-11-09
updated: 2022-11-15
---

This section helps you upgrade an extension from Manifest V2 to Manifest V3. A summary of the work is provide below with links to pages for each of the task areas. There is also a list of new extension features you should consider incorporating into your next extenion. Finally, there is a [migration checklist](/docs/extensions/mv3-migration-checklist) to help you keep track of your work.

{% Aside %}
Follow [What's new in Chrome Extensions][doc-whats-new] to read about new Manifest V3 features as they become available.
{% endAside %}

## Migration summary  {: #migration-summary }

* [Update the manifest](/docs/extensions/migrate-to-mv3/update-the-manifest/)&mdash;The `manifest.json` requires a slightly different format for V3 than for V2. Some of these changes may be made on their own. Others require parallel changes to your code.
* [Upgrade to a service worker](/docs/extensions/migrate-to-mv3/create-the-service-worker/)&mdash;A single extension service workers replace background scripts to ensure that background code stays off the main thread where it can hurt performance.
* [Update your code](/docs/extensions/migrate-to-mv3/refactor-code)&mdash;Some features need to be replaced with more modern equivalents. Other need to be removed entirely.
* [Replace blocking web requests](/docs/extensions/migrate-to-mv3/migrate-web-requests)&mdash;Blocking web requests in Manifest V2 could significantly degrade performance. Replace the `Chrome.webRequest` interface with rules specified in the manifest under `"declarative_net_request"`.
* [Improve extension security](/docs/extensions/migrate-to-mv3)&mdash;Security improvements have been made, including ending support for remotely hosted code.

## New extension features

The following is a list of new or recent extension features. You are not required to use them when upgrading; however, when they replace older features, you should prefer them to the features they replace and expect that the replaced features will be eventually be deprecated and removed.

* Many methods now return [Promises](/docs/extensions/mv3/intro/mv3-overview#promises) support has been added to many methods, though callbacks may still be passed to those methods. (We will eventually support promises on all appropriate methods.)
