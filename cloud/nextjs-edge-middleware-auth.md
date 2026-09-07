# Edge Middleware Authentication in Next.js App Router

*Date: 2026-09-07*  
*Category: Cloud & Web Architecture*

## Overview

When engineering transaction platforms like CursedCore, route security must be verified before requests hit Server Components or database tiers. Next.js Edge Middleware allows sub-10ms route evaluation at the CDN edge.

## Implementation Pattern

```typescript
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function middleware(request: NextRequest) {
  const sessionToken = request.cookies.get('session_token')?.value;
  const isProtectedPath = request.nextUrl.pathname.startsWith('/dashboard');

  if (isProtectedPath && !sessionToken) {
    const loginUrl = new URL('/login', request.url);
    loginUrl.searchParams.set('redirect', request.nextUrl.pathname);
    return NextResponse.redirect(loginUrl);
  }

  return NextResponse.next();
}

export const config = {
  matcher: ['/dashboard/:path*', '/api/admin/:path*'],
};
```

## Architectural Benefits

* **Zero Cold Starts:** Edge runtimes execute instantly globally.
* **Origin Offloading:** Unauthorized traffic is rejected before touching cloud compute or database instances.
