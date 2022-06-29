---
layout: 'layouts/blog-post.njk'
title: Why not everyone's building for the web yet, but why potentially they should
subhead: >

date: 2022-06-28
# updated: 2022-06-28
hero: image/8WbTDNrhLsU0El80frMBGE4eMCD3/8FZcBmFowbDKWxpkOytx.jpg
alt: Blowfish swarm swimming in the ocean.
tags:
  - capabilities
---

{% Aside %} This is part one of a two part mini series on the capable web. Be sure to also read
[How Fugu is my browser and how Fugu is the Web?](/blog/how-fugu/)! {% endAside %}

When you want to build an app today, you have several options to do so. You can build a
platform-specific app for the platforms you care about, like, for example, Windows, Android, and
iOS. That is, in this case, technically, you would build three apps. You can also build a
(progressive) web app, possibly _on top_ of platform-specific apps. Alternatively, you can choose a
cross-platform framework like [Electron.js](https://www.electronjs.org/) or
[Ionic](https://ionicframework.com/) that promises to let you write once and run anywhere. This very
publication being about the web, let me begin with walking you through three extraordinary examples
of apps whose makers chose to build for the web.

## Lighthouse cases that demonstrate the web's abilities

Photoshop has for the longest time been recognized as one of the last bastions of high quality apps
that supposedly would never make it to the web platform. Record scratch, this last bastion finally
fell. With [Photoshop on the web](https://web.dev/ps-on-the-web/), Adobe together with Chromium
engineering has managed to get a version of Photoshop running in the browser that can serve as the
new lighthouse showcase of what is possible on the web.

<figure>
  {% Img src="image/8WbTDNrhLsU0El80frMBGE4eMCD3/bobe9QD2jUL0hTbpnNMU.png", alt="Adobe Photoshop running in the browser.", width="800", height="500" %}
  <figcaption>
    Photoshop on the web
  </figcaption>
</figure>

Similarly, Microsoft has launched
[Visual Studio Code on the web](https://code.visualstudio.com/blogs/2021/10/20/vscode-dev), a
full-fledged, installable web experience of its IDE that makes developing completely in the browser
possible, including the option to open and edit files on the local file system.

<figure>
  {% Img src="image/8WbTDNrhLsU0El80frMBGE4eMCD3/i8DIRLsHwHeEJX4NPUUn.png", alt="Microsoft VS Code running in the browser.", width="800", height="483" %}
  <figcaption>
    VS Code on the web
  </figcaption>
</figure>

Lastly Twitter—whose PWA is
[largely seen](https://www.thurrott.com/cloud/social/150171/twitter-lite-now-adapts-larger-screens#:~:text=probably%20the%20best%20mainstream%20progressive%20web%20app)
as _probably the best mainstream progressive web app_—for a while now
[uses](https://blog.twitter.com/engineering/en_us/topics/insights/2019/twitter-for-mac-is-coming-back#:~:text=This%20led%20to%20our%20Responsive%20Web%20codebase%20being%20the%20spearhead%20for%20all%20platforms%20via%20web%20browsers)
their responsive web codebase for all platforms, mobile and desktop, via web browsers. On Windows,
the PWA is the experience the company is confident enough to make it _the_ Twitter experience that
you get when you
[install](https://blog.twitter.com/en_us/topics/product/2018/a-new-twitter-experience-on-windows)
the app from the Microsoft Store.

<figure>
  {% Img src="image/8WbTDNrhLsU0El80frMBGE4eMCD3/Fj4zLvYFKCmQQuJ1YsHQ.png", alt="Microsoft Store showing the Twitter app.", width="800", height="568" %}
  <figcaption>
    Twitter in the Microsoft Store
  </figcaption>
</figure>

## Linkability and universality, the web's super powers

All three companies, Adobe, Microsoft, and Twitter, in parallel to their web apps have
well-established platform-specific Windows, macOS, Android, iOS, Linux,… etc. versions of their apps
Photoshop, Visual Studio Code, and Twitter respectively. So why did they build for the web on top?
The answer lies in its linkability and universality.

As Google's [Thomas Nattestad](https://twitter.com/fractorious) concisely
[puts it](https://web.dev/ps-on-the-web/#:~:text=the%20simple%20power%20of%20a%20url%20is%20that%20anyone%20can%20click%20it%20and%20instantly%20access%20it.%20all%20you%20need%20is%20a%20browser.%20there%20is%20no%20need%20to%20install%20an%20application%20or%20worry%20about%20what%20operating%20system%20you%20are%20running%20on.):
_"The simple power of a URL is that anyone can click it and instantly access it. All you need is a
browser. There is no need to install an application or worry about what operating system you are
running on"_. With Visual Studio Code for the web,
[according to](https://code.visualstudio.com/blogs/2021/10/20/vscode-dev#:~:text=You%20can%20make%20quick%20edits%2C%20review%20PRs%2C%20and%20Continue%20on%20to%20a%20local%20clone)
Microsoft's [Chris Dias](https://twitter.com/chrisdias), when working with GitHub, _"you can make
quick edits, review PRs, and continue on to a local clone"_. The sole fact that you can share a link
to your work unlocks collaboration patterns that users embrace and love since the birth of apps like
Google Docs. Twitter of course lives and dies with links. News sites regularly
[link to newsworthy tweets](https://edition.cnn.com/2021/11/09/politics/gosar-anime-video-violence-ocasio-cortez-biden/index.html#:~:text=Ocasio-Cortez%20tweeted%20in%20response%20Monday%20saying%20a%20%22creepy%20member%22%20of%20the%20House%20had%20%22shared%20a%20fantasy%20video%20of%20him%20killing%20me.%22),
which _means "keeping it quick"_
[is core](https://blog.twitter.com/engineering/en_us/topics/infrastructure/2019/progressively-enhancing-desktop-devices#:~:text=%C2%A0-,Keeping%20it%20quick,-With%20all%20the)
to ensuring people can get from the article straight into the app, where they can read or engage
with the linked tweet.

Web applications are inherently universal. They run on whatever operating system capable of running
a web browser and do not need to be compiled for each operating system separately. The same code
base powers the application on all platforms. This does not mean that there are no compatibility
issues—there are plenty actually—but there is a
[solid shared increasing baseline](https://web.dev/interop-2022/) that all applications can build
upon.

## Linkability of platform-specific apps

While more ubiquitous on mobile, on desktop linking into a platform-specific app from the web is
comparatively rare. On mobile (and macOS), this works via a technology called
[Universal Links](https://developer.apple.com/ios/universal-links/) on iOS (and on macOS) and
[App Links](https://developer.android.com/training/app-links/) on Android. Platform-specific apps
alternatively can rely on
[registered protocol schemes](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app)
like, for example, `itms-apps:` if they want to deep-link into the App Store app on macOS or iOS, or
register their own custom schemes for their own apps. So while technically possible, linking into
platform-specific apps is a lot less flexible and requires more plumbing work than simply linking
into a web app.

## The slow decline of interest in cross-platform app frameworks and the rise of Flutter

The web isn't the only platform that promises "write once, run anywhere". Cross platform frameworks
à la [Electron.js](https://www.electronjs.org/) do, too. With the Web becoming powerful enough to
drive Web apps that were believed to be impossible like Photoshop, we can, however, observe a slow
decline of interest in cross-platform desktop app frameworks like
[Electron.js](https://www.electronjs.org/) and [NW.js](https://nwjs.io/) and mobile app frameworks
like [Cordova](https://cordova.apache.org/) or [React Native](https://reactnative.dev/), while at
the same time there is an undeniable increase of interest in [Flutter](https://flutter.dev/). The
[Google Trends chart](https://trends.google.com/trends/explore/TIMESERIES/1636457400?hl=en-US&tz=-60&cat=5&date=today+5-y&q=%2Fg%2F11bw_559wr,%2Fg%2F11f11js9bh,%2Fm%2F06znsr5,%2Fg%2F11f03_rzbg,%2Fg%2F11h03gfxy9&sni=3)
below shows all five side by side, well noting that this chart shows disambiguated topic trends as
detected by Google (as opposed to ambiguous
[search term trends](https://blog.tomayac.com/2021/11/08/things-mode-and-strings-mode-in-google-trends/)),
but it nevertheless is acknowledgedly not exact science.

<figure>
  <div class="glitch-embed-wrap" style="height: 425px; width: 100%;">
    <iframe src="https://trends.google.com/trends/embed/explore/TIMESERIES?req=%7B%22comparisonItem%22%3A%5B%7B%22keyword%22%3A%22%2Fg%2F11bw_559wr%22%2C%22geo%22%3A%22%22%2C%22time%22%3A%22today%205-y%22%7D%2C%7B%22keyword%22%3A%22%2Fg%2F11f11js9bh%22%2C%22geo%22%3A%22%22%2C%22time%22%3A%22today%205-y%22%7D%2C%7B%22keyword%22%3A%22%2Fm%2F06znsr5%22%2C%22geo%22%3A%22%22%2C%22time%22%3A%22today%205-y%22%7D%2C%7B%22keyword%22%3A%22%2Fg%2F11f03_rzbg%22%2C%22geo%22%3A%22%22%2C%22time%22%3A%22today%205-y%22%7D%2C%7B%22keyword%22%3A%22%2Fg%2F11h03gfxy9%22%2C%22geo%22%3A%22%22%2C%22time%22%3A%22today%205-y%22%7D%5D%2C%22category%22%3A5%2C%22property%22%3A%22%22%7D&tz=-60&forceMobileMode=false&isPreviewMode=true&eq=q%3D%252Fg%252F11bw_559wr%2C%252Fg%252F11f11js9bh%2C%252Fm%252F06znsr5%2C%252Fg%252F11f03_rzbg%2C%252Fg%252F11h03gfxy9%26date%3Dtoday%25205-y%26cat%3D5%23TIMESERIES&hl=enUS"
      title="Google Trends interest over time for Electron, NW.js, Apache Cordova, Flutter, and React Native."
      style="height: 100%; width: 100%; border: 0;">
    >
    </iframe>
  </div>
  <figcaption>
    Google Trends interest over time for Electron, NW.js, Apache Cordova, Flutter, and React Native.
    [<a href="https://www.google.com/url?q=https://trends.google.com/trends/embed/explore/TIMESERIES?req%3D%257B%2522comparisonItem%2522%253A%255B%257B%2522keyword%2522%253A%2522%252Fg%252F11bw_559wr%2522%252C%2522geo%2522%253A%2522%2522%252C%2522time%2522%253A%2522today%25205-y%2522%257D%252C%257B%2522keyword%2522%253A%2522%252Fg%252F11f11js9bh%2522%252C%2522geo%2522%253A%2522%2522%252C%2522time%2522%253A%2522today%25205-y%2522%257D%252C%257B%2522keyword%2522%253A%2522%252Fm%252F06znsr5%2522%252C%2522geo%2522%253A%2522%2522%252C%2522time%2522%253A%2522today%25205-y%2522%257D%252C%257B%2522keyword%2522%253A%2522%252Fg%252F11f03_rzbg%2522%252C%2522geo%2522%253A%2522%2522%252C%2522time%2522%253A%2522today%25205-y%2522%257D%252C%257B%2522keyword%2522%253A%2522%252Fg%252F11h03gfxy9%2522%252C%2522geo%2522%253A%2522%2522%252C%2522time%2522%253A%2522today%25205-y%2522%257D%255D%252C%2522category%2522%253A5%252C%2522property%2522%253A%2522%2522%257D%26tz%3D-60%26forceMobileMode%3Dfalse%26isPreviewMode%3Dtrue%26eq%3Dq%253D%25252Fg%25252F11bw_559wr%252C%25252Fg%25252F11f11js9bh%252C%25252Fm%25252F06znsr5%252C%25252Fg%25252F11f03_rzbg%252C%25252Fg%25252F11h03gfxy9%2526date%253Dtoday%2525205-y%2526cat%253D5%2523TIMESERIES%26hl%3DenUS&sa=D&source=docs&ust=1656333876287238&usg=AOvVaw1QYdCACMEsvrJP2BsAbyBT">Source</a>]
  </figcaption>
</figure>

This trend is backed by
[Statista stats](https://www.statista.com/statistics/869224/worldwide-software-developer-working-hours/),
according to which Flutter has passed React Native as the most popular framework.

<figure>
  {% Img src="image/8WbTDNrhLsU0El80frMBGE4eMCD3/FzxrRgok5BESHk0z4mkX.png", alt="Flutter is the most popular cross-platform mobile framework used by global developers, according to a 2021 developer survey. Based on the survey, 42 percent of software developers used Flutter. On the whole, roughly one third of mobile developers use cross-platform technologies or frameworks; the rest of mobile developers use native tools.", width="800", height="594" %}
  <figcaption>
    Cross-platform mobile frameworks used by software developers worldwide from 2019 to 2021.
    [<a href="https://www.statista.com/statistics/869224/worldwide-software-developer-working-hours/">Source</a>]
  </figcaption>
</figure>

[StackOverflow statistics](https://insights.stackoverflow.com/trends?tags=flutter%2Creact-native%2Celectron%2Cionic-framework)
on tag usage agree. The underlying assumption of people actually using a technology being correlated
with people asking questions about said technology on StackOverflow is not out of this world.

<figure>
  {% Img src="image/8WbTDNrhLsU0El80frMBGE4eMCD3/jD4FjbDxAzp3KkOyCoTV.png", alt="How technologies have trended over time based on use of their tags since 2008, when Stack Overflow was founded. Comparing the tags flutter, react-native, electron, and ionic-framework. Flutter's curve increases the steepest, with React Native being the flattening second and Ionic and Electron the flattening 'also ran'.", width="800", height="492" %}
  <figcaption>
    StackOverflow statistics on tag usage.
    [<a href="https://insights.stackoverflow.com/trends?tags=flutter%2Creact-native%2Celectron%2Cionic-framework">Source<a>]
  </figcaption>
</figure>

## Shouldn't everybody be building for the web then?

Given the examples, Photoshop, VS Code, and Twitter, that it indeed is possible to build amazing
applications on the web and given the web's super powers of linkability and universality, then
_(1)_, why do people still not fully bet on PWAs but build platform-specific apps and _(2),_ do at
least in part happily flock to Flutter?

## Why do people not fully bet on PWA?

For _(1)_, the answer is complex and manyfold. I want to break it down into different
sub-categories.

### Missing capabilities, aka. the app gap

Web applications still lack certain functionalities that platform-specific apps have. In the
following, I list representative examples of such functionalities on different platforms. First, it
is impossible even with an installed PWA to obey the macOS user interface paradigm of having the
[app menu at the top of the screen](https://developer.apple.com/design/human-interface-guidelines/macos/menus/menu-bar-menus/).
It can easily be achieved with frameworks like Electron.js via the
<code>[Menu](https://www.electronjs.org/docs/latest/api/menu)</code> class. (On the web, the next
best thing is [Window Controls Overlay](https://web.dev/window-controls-overlay/) and getting
support for app menus is tracked as [crbug/1295253](https://crbug.com/1295253).) Another example are
in-app purchases on macOS that can be handled via Electron's
<code>[inAppPurchase()](https://www.electronjs.org/docs/latest/tutorial/in-app-purchases)</code>
method. (On the web, the next best thing is the
[Digital Goods API](/docs/android/trusted-web-activity/receive-payments-play-billing/), currently
limited to Android and Chrome OS.) Installers are a common way users have learned to install
applications on Windows. With Electron.js, it is possible to create
[installers](https://www.electronjs.org/docs/latest/api/auto-updater#windows) and make installed
applications [update automatically](https://www.electronjs.org/docs/latest/api/auto-updater). (On
the web, [Web Bundles](https://web.dev/web-bundles/) are the next best alternative in Chrome.) This
list is non-exhaustive, and Electron.js is mentioned as a representative framework out of many.

<details>
  <summary>How big of a challenge is this?</summary>
  <p>
    There're undeniably a number of capabilities that are missing from the web. In many cases, they are
    nice-to-have, but not necessarily required for a still great experience. Carefully assess if a
    capability can be seen as a progressive enhancement.
  </p>
</details>

### Discoverability in stores

Collectively we have educated users to look for apps in app stores. Some stores like the
[Windows Store](https://developer.microsoft.com/en-us/microsoft-store/pwa/) and the
[Android Play Store](/docs/android/trusted-web-activity/quick-start/) have started to embrace
(wrapped!) PWAs (optionally
[limited to Chrome OS](https://chromeos.dev/en/publish/pwa-in-play#chrome-os-only)) and offer
graphical user interface tools like [PWABuilder](https://www.pwabuilder.com/) (internally based on
the command line tool [bubblewrap](https://github.com/GoogleChromeLabs/bubblewrap)) for submitting
applications. Meanwhile on other stores like Apple's App Store, the situation is different and less
welcoming, and apps
[may or may not](https://blog.pwabuilder.com/posts/publish-your-pwa-to-the-ios-app-store/) make it
into the store, depending on the outcome of the app review. Recently, Oculus, a division of Meta
Platforms that produces virtual reality headsets, has announced that PWAs would be accepted on the
[Oculus Store](https://developer.oculus.com/pwa/).

<details>
  <summary>How big of a challenge is this?</summary>
  <p>
    If your users are on one of the platforms whose stores accept PWAs, you can just publish your app
    to the stores in question. Else, remember linkability as one of the web's super powers. Your app
    is discoverable, advertisable, and linkable from the web, too.
  </p>
</details>

### Monetization of apps and in-app content

Apart from making apps themselves available for a fee, apps can also be monetized by selling items
as in-app purchases (for example, items in a game app) or by selling subscriptions (for example,
regular courses in a fitness app). If the developer integrates with payment providers, all of this
is available to web apps as well, but the smooth integration of stores and their related payment
systems makes this a lot more attractive for platform-specific apps, albeit at a 15–30% commission.
For apps built using [Trusted web Activities](/docs/android/trusted-web-activity/) and delivered
through the Google Play Store, developers can now use the
[Payment Request API](https://developer.mozilla.org/docs/Web/API/Payment_Request_API) and the new
[Digital Goods API](/docs/android/trusted-web-activity/receive-payments-play-billing/) to integrate
with Google Play Billing.

<details>
  <summary>How big of a challenge is this?</summary>
  <p>

  </p>
</details>

### Hiring or retraining developers

From personal experience from talking to many of Google's partners, a lot of
[companies struggle with hiring great web developers](https://medium.com/javascript-scene/why-hiring-is-so-hard-in-tech-c462c3230017).
The talent shortage is real, and recruiting costs are high, which is why startups commonly early-on
hire in-house recruiters who oftentimes approach recruiting with a shotgun approach that has not
helped the guild's reputation with IT professionals. Companies also often already employ teams of
Android and/or iOS developers that they cannot just retrain to become web developers. Creating a PWA
requires a high level of specialization that not all web developers can offer.

<details>
  <summary>How big of a challenge is this?</summary>
  <p>

  </p>
</details>

### Legacy existing apps (and migrating its user base)

It is not unusual for companies to have made massive investments in platform-specific apps, and
giving up these investments as well as the over the time acquired user base (not to speak of the
vanity install statistics) is not easy. Seemingly starting from scratch, even when the companies
have an existing website, appears very unattractive in comparison, but sometimes it
[happens](https://www.glossy.co/ecommerce/gone-fishin-patagonia-bids-farewell-to-mobile-app/).

<details>
  <summary>How big of a challenge is this?</summary>
  <p>

  </p>
</details>

### Compatibility with relevant browsers

Web compatibility is still the
[top issue](https://insights.developer.mozilla.org/reports/mdn-web-developer-needs-assessment-2020.html#needs-assessment-top-ten-needs)
mentioned in developer surveys like Mozilla's, but also internal surveys Google has run. Having to
support specific browsers, avoiding or removing a feature that doesn't work across browsers, or
making a design look or work the same across browsers are frequently brought up challenges. Projects
like [webcompat.com](https://webcompat.com/) collect user-submitted browser bugs and invite
interested developers to fix them. Mozilla operates a
[repository](https://github.com/mozilla-extensions/webcompat-addon) with interventions and patches
to enable individual sites to run successfully in Firefox. WebKit maintains a
[quirks list](https://trac.webkit.org/browser/webkit/trunk/Source/WebCore/page/Quirks.cpp) and
[hires WebKit Web Compatibility Analysts](https://jobs.apple.com/search?search=%22WebKit%20Web%20Compatibility%20Analyst%22&sort=relevance).
Most importantly, though, the browser vendors themselves have joint forces to improve web
compatibility in core areas in the context of the [Compat 2021](https://web.dev/compat2021/) and the
[Interop 2022](https://web.dev/interop-2022/) efforts, with
[Interop 2023](https://github.com/web-platform-tests/interop-2022/issues/78) already in the
planning.

<details>
  <summary>How big of a challenge is this?</summary>
  <p>

  </p>
</details>

### Tools and framework support

Mozilla's
[developer survey](https://insights.developer.mozilla.org/reports/mdn-web-developer-needs-assessment-2020.html#needs-assessment-top-ten-needs),
apart from browser compatibility, likewise showed that developers struggle with tools and
frameworks. Supporting multiple frameworks in the same code base, understanding and implementing
security measures, plus outdated or inaccurate documentation for frameworks and libraries, and
keeping up with a large number of new and existing tools or frameworks were mentioned.

<details>
  <summary>How big of a challenge is this?</summary>
  <p>

  </p>
</details>

### Security (or rather, security theater with certificate pinning)

In platform-specific app development, certificate pinning restricts which certificates are
considered valid for a particular app. Instead of allowing any trusted certificate to be used,
developers pin the certificate authority issuer, public keys, or even end-entity certificates of
their choice. Clients connecting to that server will treat all other certificates as invalid and
refuse to make an HTTPS connection. The hope is that this renders person-in-the-middle attacks
impossible, so platform-specific apps are more "secure" than web apps, where traffic can easily be
sniffed with browser DevTools. There are ways to
[circumvent pinned certificates](https://codeshare.frida.re/@akabe1/frida-multiple-unpinning/) on
all platforms, so it is mostly a security theater at this point.

<details>
  <summary>How big of a challenge is this?</summary>
  <p>

  </p>
</details>

### Performance limitations

Web applications have seen impressive performance improvements thanks to advanced technologies like
[Web Assembly](https://webassembly.org/) (including [SIMD](https://v8.dev/features/simd)),
[WebGPU](https://gpuweb.github.io/gpuweb/), and just general JavaScript engine progress in recent
years. Nonetheless will a carefully developed platform-specific app typically outperform a web-based
application, albeit the situations where this _actually_ matters may be limited. With even
high-performance audio editing tools like [Soundtrap](https://www.soundtrap.com/) (thanks to the
[Web Audio API](https://developer.mozilla.org/docs/Web/API/Web_Audio_API) and
[AudioWorklet](https://developer.mozilla.org/docs/Web/API/AudioWorklet)), interactive development
environments like [Jupyter Notebook](https://jupyter.org/try) and graphics editing tools like
[Figma](https://www.figma.com/) (thanks to Web Assembly), and of course graphics-intensive games
like [Quake](http://www.quakejs.com) (thanks to
[WebGL](https://developer.mozilla.org/de/docs/Web/API/WebGL_API) and
[WebGPU](https://gpuweb.github.io/gpuweb/) in the future), the boundary is pushed at a rapid rate.

<details>
  <summary>How big of a challenge is this?</summary>
  <p>

  </p>
</details>

## Why is Flutter so popular?

For _(2)_, one possible explanation is that it is a
[Google-backed](https://flutter.dev/#:~:text=Flutter%20is%20Google%27s%20UI%20toolkit%20for%20building%20beautiful%2C%20natively%20compiled%20applications%20for%20mobile%2C%20web%2C%20desktop%2C%20and%20embedded%20devices%20from%20a%20single%20codebase.)
toolkit for _"building beautiful, natively compiled applications for mobile, Web, desktop, and
embedded devices from a single codebase"_. If even Google, as the maker of Android, trusts Flutter
enough to build some of its strategic apps like
[Stadia](https://stadia.dev/blog/how-flutter-helped-us-make-stadia-controller-setup-better-for-users/)
and [Google Ads](https://flutter.dev/showcase) with it for both Android and iOS, and
[Assistant apps](https://developers.googleblog.com/2019/05/Flutter-io19.html) on smart display
embedded devices, that is quite a signal to send. Also note how web and desktop are included in
Flutter's output options, which means Flutter is no longer limited to just mobile (with submission
into app stores as the carrot), and the promise is that it reduces the development cost of apps by
the number of targeted platforms. (Prominent target platform omissions so far are Apple CarPlay,
WearOS, WatchOS, and tvOS.)

An argument that is frequently brought up for Flutter is
[hot reloading](https://flutter.dev/docs/development/tools/hot-reload). On the backend, Flutter also
[plays well with Firebase](https://firebase.google.com/docs/flutter/setup?platform=ios), so apps are
easy to scale. Important for web, and since Flutter initially was criticized for rendering
everything inaccessibly onto a `<canvas>`, the framework now has
[two different web renderers](https://flutter.dev/docs/development/tools/web-renderers) it can
optionally automatically choose from:

- **HTML renderer:** This renderer uses a combination of HTML elements, CSS, canvas elements, and
  SVG elements and has a smaller download size.
- **CanvasKit renderer:** This renderer is fully consistent with Flutter mobile and desktop, has
  faster performance with higher widget density, but adds about 2 MB in download size.

By default, Flutter selects the HTML renderer when the app is running in a mobile browser, and the
CanvasKit renderer when the app is running in a desktop browser.

Flutter relies on [a library of pre-made widgets](https://docs.flutter.dev/development/ui/widgets)
called Cupertino (for the iOS-native look) and Material (for the Android-native look) that allows
developers to quickly develop a good-looking application with a shared code base. It is well worth
noting that Flutter-built user interfaces are platform-agnostic because Flutter’s
[Skia](https://skia.org/) rendering engine does not require any platform-specific UI components. (A
downside of this approach of wrapping everything the app needs instead of reusing platform
primitives directly is app size.)

Apps in Flutter are developed in Dart, an object-oriented programming language that supports both
just-in-time (JIT) and ahead-of-time (AOT) compilation and compiles directly to native ARM or Intel
x64 code, which has a lot of performance advantages. Dart is also easy to pick up for developers
coming from any other object-oriented programming language.

Flutters [documentation](https://flutter.dev/docs) is generally recognized as best in class and its
[cookbook application](https://docs.flutter.dev/cookbook) makes getting started with a baseline
scaffolding a simple copy and paste job. The Flutter community is thriving and it's easy to find
help when one is stuck.

<figure>
  {% Img src="image/8WbTDNrhLsU0El80frMBGE4eMCD3/E022tgznha53Y5ghQoT3.png", alt="Blue cartoon bird.", width="800", height="450" %}
  <figcaption>Dash, the mascot for the Dart language and the Flutter framework.</figcaption>
</figure>

## Conclusions

It is undeniable that amazing apps can be built on the web. Photoshop, VS Code, and Twitter were the
leading examples in this article, but there are many others. The web's super power is its
linkability, which is hard to beat on platforms other than the web. There seems to be a certain
tendency of cross-platform app frameworks becoming less attractive to developers, with the notable
exception being Flutter, which allows for web as one of its target platforms. Reasons for not
building for the web are not hard to find, but it is likely not hard to find counterarguments to
take these reasons apart. Some of them rely on outdated or weak assumptions, for example, PWAs being
not welcome on app stores or platform-specific apps being more secure than PWAs. Others are things
that are in process, like closing the app gap by adding missing web platform APIs. Some reasons
apply equally to both worlds, for example, hiring being a challenge. I could go on and on, but in
the end it all boils down to the concrete circumstances your use case needs to be built for. In this
article, I have given a number of really strong arguments for building for the web, while also not
hiding the fact that the web is a platform that is still not perfect and that other alternatives
exist. And as the three leading examples have shown, the decision is also not mutually exclusive.
You can build a powerful PWA for the web, and also have a great platform-specific application at the
same time. It is up to you to decide if you have to.

{% Aside %} This is part one of a two part mini series on the capable web. Be sure to also read
[How Fugu is my browser and how Fugu is the Web?](/blog/how-fugu/)! {% endAside %}