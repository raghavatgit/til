# ASGI Lifespan Context Managers in Modern FastAPI

*Date: 2026-09-13*  
*Category: Backend Architecture / Cloud*

## Overview

In modern FastAPI / Starlette applications, the legacy `@app.on_event("startup")` and `@app.on_event("shutdown")` decorators are deprecated. Modern applications manage resource lifecycles (database connection pools, telemetry serial bridges, ML models) using the standard `asynccontextmanager` lifespan protocol.

## Implementation Pattern

```python
from contextlib import asynccontextmanager
from fastapi import FastAPI
import httpx

# Shared resources container
state = {}

@asynccontextmanager
async def lifespan(app: FastAPI):
    # --- Startup: Initialize connection pools ---
    state["http_client"] = httpx.AsyncClient(timeout=10.0)
    print("Telemetry connection pool initialized.")
    
    yield  # Application processes requests here
    
    # --- Shutdown: Graceful resource drainage ---
    await state["http_client"].aclose()
    print("Telemetry connection pool drained cleanly.")

app = FastAPI(lifespan=lifespan)

@app.get("/health")
async def health_check():
    return {"status": "healthy", "client_active": not state["http_client"].is_closed}
```

## Architectural Advantages
* **Guaranteed Cleanup:** Code following `yield` is guaranteed to execute even if the application encounters uncaught startup exceptions.
* **Unified Scope:** Local variables declared prior to `yield` remain accessible in the shutdown phase without fragile global state mutation.
