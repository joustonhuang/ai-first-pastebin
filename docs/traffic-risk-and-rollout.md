# Traffic risk and rollout notes

## Current hosting profile

- Single Oracle VPS
- FastAPI main app + FastMCP sidecar
- SQLite backing store
- Public unauthenticated endpoint
- Upload support can be materially heavier than read/resolve traffic

## Main failure modes if directory traffic lands quickly

1. **Write amplification on SQLite**
   - many create/resolve requests can increase lock contention
   - chain/tag lookup growth may make slow queries visible earlier than expected

2. **Upload abuse / burst pressure**
   - current documented limits allow large request bodies
   - agent ecosystems can accidentally retry failed uploads aggressively

3. **Unbounded anonymous use**
   - marketplace discovery can turn a handy niche tool into a general public utility overnight
   - crawlers and benchmark bots may call every tool automatically

4. **MCP transport fan-out**
   - some clients probe tools repeatedly during capability discovery
   - HTTP/SSE style clients may reconnect frequently

## Minimum safeguards before broad promotion

- nginx rate limiting per IP and a stricter zone for `/mcp` and upload routes
- cap concurrent upstream connections to the MCP process
- request timeout and body-size review specific to MCP tools
- metrics for RPS, 95p latency, 5xx rate, SQLite busy/lock events, disk use, and outbound traffic
- simple circuit breaker or maintenance mode for write-heavy tools
- cacheable responses for read-mostly metadata endpoints where safe

## Recommended rollout order

### Phase 1
- publish repo
- publish directory entries
- monitor closely
- be ready to disable write-heavy tools temporarily if traffic is abnormal

### Phase 2
- add rate limiting + observability
- add explicit usage policy in README/llms docs
- consider soft auth or per-client keys for higher-volume use

### Phase 3
- if sustained usage appears, move writes off SQLite or isolate them behind a queue/service boundary

## Product recommendation

Discovery is worth doing now, but I would treat this as a **guarded soft launch**, not a fully open floodgate.
