## Overview
This case uses a vulnerability in Mojo IPC (interprocess communication) mechanisms to escape the Chrome sandbox on Android. The exploit is tracked as CVE-2021-30528

## Sources
-  [Technical write-up](https://github.blog/security/vulnerability-research/the-fugitive-in-java-escaping-to-java-to-escape-the-chrome-sandbox/)
- [Patch Notes](https://chromereleases.googleblog.com/2021/05/stable-channel-update-for-desktop_25.html)

## Starting Conditions
- A prerequisite is that the user has a credit card saved to their Google account
- Attacker needs an initial renderer compromise
- Using Android Google Chrome 64-bit

## Escape Summary
The vulnerability here is that C++ and Java have different object lifetimes. Chrome on Android uses both Java and C++. The problem is that Java pointed to a C++ object, and the pointer remained after the C++ object got destroyed, so it was a use after free vulnerability. Then the attacker uses heap spray and the Chrome clipboard to place data in that cleared out pointer location. This data does a callback to a predictable memory address, which the attacker can put arbitrary code into and execute it. 

## Classification
- Channel: Browser IPC
- Bug type: Use after Free
- Boundary crossed: Renderer → Browser process
- Protection failure: Object lifetime handling failure
- Bug count: 1

## Fix
[Commit notes](https://chromium-review.googlesource.com/c/chromium/src/+/2892421)
The fix included making the pointer that was changed mutable, then making a method to invalidate that pointer. Then they call that method to run it. 
