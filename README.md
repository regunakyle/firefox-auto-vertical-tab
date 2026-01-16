# Firefox Auto Vertical Tab Toggle

<video src="https://github.com/user-attachments/assets/cc06ef4b-3996-448d-827a-85216857767d"  controls></video>

This is a Firefox ONLY extension built with [WXT](https://wxt.dev/).

Requires Firefox 142 or newer.

This extension does not collect any data nor send any data to any server.

<https://x.com/bramus/status/2012094220627071117?s=20>

## Logic

Check the width of currently focused Firefox window every 0.1 second:

1. If width <= user defined threshold, enable vertical tab
2. If width > user defined threshold, disable vertical tab

The user defined threshold is 1080 (in pixels) by default.

You may also choose to reverse this behavior, so that vertical tabs are enabled when the window width is above the threshold (and vice versa).

You can edit these settings in the extension's menu.

## Why use an interval instead of the window.onresize event listener?

The `window.onresize` event listener is indeed the most obvious solution to the problem.

However, I can only use it within content scripts, which do not work in *privileged* domains (e.g. `about:config`, [Mozilla domains](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Content_scripts#restricted_domains)).

In addition, requiring a content script means the extension would need access to all domains. This might discourage some users from installing it.

Weighing these points, I decided to use an interval in a background script instead. Checking 10 times per second is not much of a burden for modern PCs anyway.

In fact, there **is** a `windows.onBoundsChanged` event listener ([MDN](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/windows/onBoundsChanged)) that can work in background script. The problem is that Firefox hasn't implement it (lol).

Please leave a comment in this [Bugzilla](https://bugzilla.mozilla.org/show_bug.cgi?id=1762975) to nudge them to implement it. In the meantime, please also upvote this [per-window vertical tab setting feature request](https://connect.mozilla.org/t5/ideas/vertical-tabs-per-window/idi-p/94064).

## Build

### Environment

- Fedora 42 KDE
- Firefox Developer Edition (142.0b5)
- Node.js 22.17.1

### Instructions

```bash
npm install
npm run zip
```

## TODO

- Write Playwright tests

## Attribution

The app icon (created by Smashicons) is downloaded from [flaticon.com](https://www.flaticon.com/free-icon/network_660484) for free.
