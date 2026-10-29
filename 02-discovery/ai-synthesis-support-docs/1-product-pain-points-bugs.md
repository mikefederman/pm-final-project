# User Pain Points & Typical Architecture Bugs

*All research data in this document is simulated.*

**Context:** Engineers perform single lookups in the legacy UI, which is the sanctioned way to retrieve part data. No sanctioned bulk or automated path exists. A subset of engineers, together with the collaborators who build their scripts, use shadow workarounds to reach BOM scale. The pain points below describe that subset.

## User Pain Points

- **Manual Looping Exhaustion:** Engineers using shadow workarounds, and the collaborators who build their scripts, must write custom script wrappers or execute hundreds of sequential GET requests one by one to cross-reference an inventory bill of materials (BOM), drastically slowing delivery velocity.
- **API Rate Limiting & Throttling:** Running high-frequency, single-click lookups in rapid succession frequently triggers the current architecture's rate limiters, causing scripts to fail halfway through a large run.
- **Payload Bloat & Token Waste:** The existing endpoint returns a massive, frontend-optimized JSON structure containing heavy UI layout data, image URLs, and styling strings, which wastes valuable token context windows when passed into enterprise AI pipelines.
- **Brittle Error Recovery:** If request number 450 out of 500 fails during a manual loop, engineers and the collaborators who build their scripts lack a native way to resume, forcing them to either restart the entire batch or write complex error-handling retry logic.
- **Security Red-Tape & Shadow Workarounds:** Because there is no sanctioned bulk or automated path, engineers resort to scraping data from the legacy UI or sharing unencrypted CSV dumps over insecure channels to get their jobs done, compromising data governance.

## Typical Architecture Bugs

- **N+1 Query Database Degradation:** The backend architecture executes a separate, isolated database query for every single part request, causing severe database CPU spikes and connection pool exhaustion under heavy sequential load.
- **Thread Contention & Thread-Locking:** Running parallelized single-request loops from multiple script environments strains the server's thread allocation, causing concurrent user requests to hang or drop completely.
- **Memory Leaks on Large Payloads:** Processing massive numbers of individual part objects without stream-parsing causes the server-side memory footprint to scale linearly, leading to occasional Out-Of-Memory (OOM) crashes.
- **Stale or Cached Out-of-Sync Data:** High-frequency sequential hits frequently bypass or overwhelm local application caches, resulting in the API returning inconsistent, stale, or conflicting part data across rapid consecutive calls.
- **Uncompressed Payload Latency Spikes:** The current endpoint lacks dynamic GZIP or Brotli compression for raw text, causing severe network latency and timeout failures when shadow-workaround scripts and AI pipelines pull thousands of fields across the network.
