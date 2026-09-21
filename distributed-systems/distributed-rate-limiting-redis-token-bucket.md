# Distributed Systems: Atomic Token Bucket Rate Limiting with Redis and Lua

## The Race Condition in Multi-Key Rate Limiting
Implementing rate limiting with separate Redis `GET` and `SET` calls exposes race conditions:
- Two concurrent requests both read token count 1, decrement to 0, and allow both requests, violating rate limits.

## Atomic Lua Script Execution
Redis executes Lua scripts atomically in a single thread without locking:
```lua
local key = KEYS[1]
local capacity = tonumber(ARGV[1])
local refill_rate = tonumber(ARGV[2])
local now = tonumber(ARGV[3])
local requested = tonumber(ARGV[4])

local state = redis.call('HMGET', key, 'tokens', 'last_refill')
local tokens = tonumber(state[1]) or capacity
local last_refill = tonumber(state[2]) or now

-- Compute refilled tokens based on time delta
local elapsed = math.max(0, now - last_refill)
tokens = math.min(capacity, tokens + elapsed * refill_rate)

if tokens >= requested then
    tokens = tokens - requested
    redis.call('HMSET', key, 'tokens', tokens, 'last_refill', now)
    redis.call('EXPIRE', key, math.ceil(capacity / refill_rate))
    return 1 -- Allowed
else
    redis.call('HMSET', key, 'tokens', tokens, 'last_refill', now)
    return 0 -- Rate limited
end
```
Guarantees strictly thread-safe, distributed rate enforcement with bounded memory.
