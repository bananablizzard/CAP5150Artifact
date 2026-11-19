## Overview
This is an article about using Windows kernel exploits to escape the Chrome sandbox. The vulnerability was assigned CVE-2023–21674, a user after free vulnerability. 
## Sources
- [Technical write-up](https://theori.io/blog/chaining-n-days-to-compromise-all-part-2-windows-kernel-lpe-a-k-a-chrome-sandbox-escape)
- [Patch](https://msrc.microsoft.com/update-guide/en-US/advisory/CVE-2023-21674)

## Starting Conditions
- Code execution in Chrome renderer 
- Chrome running in a vulnerable Windows version
- Ability to make Windows system calls
- Chrome `showOpenFilePicker()` API

## Escape Summary

1. Create an I/O object
2. Create a WorkerFactory object
3. Create ALPC port handle
4. Make ALPC message with synchronous-request flag
5. Call `NtWaitForWorkViaWorkerFactory` from a separate thread
6. Terminate that thread, causing a dangling pointer
7. Close the ALPC port
8. Windows accesses the pointer and triggers the UAF

## Classification
- Channel: Operating System Inter-process communication
- Bug type: Use after free
- Boundary crossed: Renderer → Windows Kernel
- Protection failure: Missing Flag Validation
- Bug count: 1

## Fix
The patch is fairly simple. In the example in the article, Microsoft changed `NtWaitForWorkViaWorkerFactory` to reject requests that contain the `ALPC_MSGFLG_SYNC_REQUEST` flag by returning an error. 
