# Window Cloaking with SetWindowDisplayAffinity

*Date: 2026-09-07*  
*Category: Systems Programming / Windows API*

## Overview

When developing desktop privacy tools like StreamShield, concealing specific application windows from capture tools (OBS, Discord, Teams) without interfering with the local monitor display requires kernel-level display affinity modification.

## Windows API Implementation

The Windows User32 function `SetWindowDisplayAffinity` controls where a window's contents are rendered:

```c
#include <windows.h>

// WDA_EXCLUDEFROMCAPTURE was introduced in Windows 10 Version 2004 (Build 19041)
#ifndef WDA_EXCLUDEFROMCAPTURE
#define WDA_EXCLUDEFROMCAPTURE 0x00000011
#endif

BOOL CloakWindow(HWND hwnd) {
    return SetWindowDisplayAffinity(hwnd, WDA_EXCLUDEFROMCAPTURE);
}

BOOL RestoreWindow(HWND hwnd) {
    return SetWindowDisplayAffinity(hwnd, WDA_NONE);
}
```

## Key Architectural Insights

1. **Zero Frame Copying:** Unlike virtual display drivers that intercept and duplicate pixel buffers, display affinity flags instruct the Desktop Window Manager (DWM) compositor directly.
2. **Local Monitor Preservation:** The window remains 100% visible, hardware-accelerated, and interactive to the user on physical screens.
3. **Capture Exclusion:** DXGI desktop duplication and Windows Graphics Capture (WGC) pipelines receive a blacked out or transparent rectangle for the specified window handle.
