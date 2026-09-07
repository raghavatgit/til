# Top-Level Process Enumeration via User32

*Date: 2026-09-07*  
*Category: Systems Programming / Windows API*

## Overview

To allow users to select applications for privacy shielding, StreamShield must enumerate running desktop applications while filtering out invisible background processes, system tooltips, and ghost windows.

## Filtering Strategy

A clean desktop window scanner relies on three primary filters:

1. `IsWindowVisible(hwnd)`: Excludes windows that are hidden or minimized to tray.
2. Window Style Check: Inspect `WS_EX_TOOLWINDOW` and `WS_POPUP` attributes to skip tooltip and overlay windows.
3. Title Length Verification: Call `GetWindowTextLengthW(hwnd) > 0` to ignore headless background window handles.

```rust
use windows::Win32::UI::WindowsAndMessaging::{EnumWindows, IsWindowVisible, GetWindowTextW};
use windows::Win32::Foundation::{HWND, LPARAM, BOOL};

unsafe extern "system" fn enum_windows_proc(hwnd: HWND, lparam: LPARAM) -> BOOL {
    if !IsWindowVisible(hwnd).as_bool() {
        return BOOL(1);
    }
    
    let mut title: [u16; 512] = [0; 512];
    let len = GetWindowTextW(hwnd, &mut title);
    if len > 0 {
        let title_str = String::from_utf16_lossy(&title[..len as usize]);
        // Register active application
    }
    
    BOOL(1)
}
```
