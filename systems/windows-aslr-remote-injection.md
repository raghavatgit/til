# 64-bit ASLR Remote DLL Injection Mechanics

*Date: 2026-09-13*  
*Category: Systems Programming / Windows API*

## Overview

Injecting dynamic libraries into 64-bit Windows applications requires resolving `LoadLibraryW` across Address Space Layout Randomization (ASLR) boundaries. Because system DLLs (`kernel32.dll`, `ntdll.dll`) are mapped to identical base addresses across all processes in a given boot session on Windows, local addresses can be safely passed to target processes.

## Injection Pipeline

1. **Target Handle Acquisition:** `OpenProcess(PROCESS_CREATE_THREAD | PROCESS_VM_OPERATION | PROCESS_VM_WRITE, FALSE, pid)`.
2. **Virtual Memory Allocation:** `VirtualAllocEx(hProc, NULL, dllPathLen, MEM_COMMIT | MEM_RESERVE, PAGE_READWRITE)`.
3. **Payload Write:** `WriteProcessMemory(hProc, pRemoteBuf, dllPath, dllPathLen, NULL)`.
4. **Remote Thread Spawning:**
   ```c
   LPVOID pLoadLibrary = (LPVOID)GetProcAddress(GetModuleHandleW(L"kernel32.dll"), "LoadLibraryW");
   HANDLE hThread = CreateRemoteThread(hProc, NULL, 0, (LPTHREAD_START_ROUTINE)pLoadLibrary, pRemoteBuf, 0, NULL);
   ```

## ASLR Uniformity Invariant
Windows optimizes physical memory by mapping shared system DLL code sections (`kernel32.dll`) to identical virtual addresses across all user processes per boot. Thus, `GetProcAddress(..., "LoadLibraryW")` in the injector process remains valid inside the target 64-bit address space.
