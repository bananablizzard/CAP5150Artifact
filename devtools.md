## Overview
The devtools exploit has to do with WebUIs like `chrome://policy` which can call JavaScript. The researcher reported the exploits and was assigned CVE-2024-6778 & CVE-2024-5836
## Sources
- [Technical write-up](https://ading.dev/blog/posts/chrome_sandbox_escape.html)
- Multiple patches linked in "Google's Response" section
## Starting Conditions
- Malicious chrome extension installed
- Victim opens Chrome DevTools
- Vulnerable build
- Chromium based browser
## Escape Summary
The researcher had made a malicious chrome extension which runs when DevTools is opened. Then the DevTools API is exploited with reload requests and the renderer crash behavior. Then JavaScript is executed on `chrome://policy` to cross the privilege boundary for WebUI. Then they enable Browser Switcher which is meant to accomodate Internet Explorer, and then they configured it to run xcalc via a shell command, which could then be changed to anything. 

## Classification
- Channel: DevTools API/WebUI
- Bug type: Authentication/Validation
- Boundary crossed: Extension → privileged WebUI
- Protection failure: Authorization and origin validation
- Bug count: 2

## Fix
## Google's Response
Directly quoted from the website: 

After I reported the vulnerability, Google quickly confirmed it and classified it as P1/S1, which means high priority and high severity. Over the next few weeks, the following fixes were implemented:
- [Adding a `loaderId` argument to the `Page.reload` command](https://chromium-review.googlesource.com/c/chromium/src/+/5542082) and [checking the `loaderID` on the renderer side](https://chromium-review.googlesource.com/c/chromium/src/+/5625857) - This ensures that the command is only valid for a single origin and won't work if the command reaches a privileged page unintentionally.
- [Checking for the URL in the `inspectedWindow.reload()` function](https://chromium-review.googlesource.com/c/devtools/devtools-frontend/+/5546062) - Now, this function isn't dependent on only the extension API revoking access.
- [Checking if the test policies are enabled in the WebUI handler](https://chromium-review.googlesource.com/c/chromium/src/+/5679162) - By adding a working check in the handler function, this prevents the test policies from being set entirely.
