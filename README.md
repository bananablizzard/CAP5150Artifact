# Browser Security Mechanisms: Sandboxing

A comparative case study of three publicly documented Chrome sandbox
escape cases.

**Author:** Joshua Bee
**Institution:** University of Central Florida

## Project Overview
This project examines how vulnerabilities in less privileged code can allow attackers to cross browser or OS security boundaries. 

The study compares three cases involving: 
- Chrome Mojo IPC
- Windows Advanced Local Procedure Call (ALPC)
- Chrome DevTools and privileged browser functionality

For the analysis I used publicly available disclosures of CVE's, vendor advisories and patches where available. 

## Research Questions

1. Which communication channels enable the escape?
2. What vulnerability types and trust boundaries are involved?
3. How many distinct vulnerabilities are required after the initial renderer compromise?

## Scope and Limitations

- The study examines three selected cases, not a trend for all Chrome sandbox escapes.
- The cases differ in operating system used, privileges, and other prerequisites.
- Findings depend on the documentation and its detail and accuracy
- No exploit is reproduced
- The comparison does not establish which platform is safest

## Sources
[1] ading2210. 2024. Escaping the Chrome Sandbox Through DevTools. Accessed:2026-08-30. https://ading.dev/blog/posts/chrome_sandbox_escape.html \
[2] Frontier Squad. 2024. Chaining N-days to Compromise All: Part 2 —Windows Kernel LPE (a.k.a Chrome Sandbox Escape). Accessed: 2026-08-30. https://theori.io/blog/chaining-n-days-to-compromise-all-part-2-windows-kernel-lpe-a-k-a-chrome-sandbox-escape \
[3] Man Yue Mo. 2021. The fugitive in Java: Escaping to Java to escape the Chrome sandbox. Accessed: 2026-08-30. https://github.blog/security/vulnerability-research/the-fugitive-in-java-escaping-to-java-to-escape-the-chrome-sandbox/

Note:
No exploit code was executing while researching and writing this paper
