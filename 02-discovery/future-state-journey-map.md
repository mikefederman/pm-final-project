# Future State Journey: Batch Retrieval for Equipment Review Workflows

**Persona:** Equipment review engineer, an engineer who verifies parts for equipment reviews and is under delivery pressure. The engineer starts the workflow, which may be built in collaboration with others.

**Goal:** Complete a BOM-wide equipment review on schedule without manually looking up each part.

*Assumptions: All research data is simulated. The feature is piloted with a select team of engineers vetted for security. Role-based access control, activity logging and reporting are assumed, not built. Full security guardrails are required before general production release.*

## Journey Timeline

**1. Submit → 2. Retrieve → 3. Analyze → 4. Deliver**

### 1. Submit
- **User Action:** Start the workflow, which submits up to 1,000 part numbers (target) in one authenticated request → no sequential loops
- **Internal State:** Confident the sanctioned path works → no urge to improvise a workaround
- **Pain Point Addressed:** Replaces custom wrappers → no hundreds of sequential GETs

### 2. Retrieve
- **User Action:** Receive all results in under 1,500 ms (target) → review starts immediately
- **Internal State:** Trusts the system is fast → stays focused on engineering judgment
- **Pain Point Addressed:** One batch call avoids rate limits → no halfway failures
- **Pain Point Addressed:** Invalid part numbers are flagged without failing the batch → no full restart after a mid-run failure

### 3. Analyze
- **User Action:** Pass lean, UI-free payload to AI agent → ≥75% fewer tokens (target)
- **Internal State:** Empowered, as AI cross-references the BOM → effort shifts to decisions
- **Pain Point Addressed:** Strips layout, image URLs, styling → ends payload bloat and token waste

### 4. Deliver
- **User Action:** Finish BOM-wide equipment review on schedule → deadline met without workarounds
- **Internal State:** Secure and compliant → no insecure CSV dumps to explain
- **Pain Point Addressed:** Solves the single-lookup limit → removes the need for scraping data from the legacy UI and CSV dumps; security visibility assumed through existing activity logging and reporting

## Competitive Advantages Over the Shadow Workaround

1. **Speed:** 1,000 lookups in under 1,500 ms (target) → replaces hundreds of sequential requests
2. **Security:** Sanctioned path → removes the need for shadow workarounds; visibility assumed through existing activity logging and reporting
3. **Cost:** Stripped, AI-ready payloads → ≥75% lower AI computing cost (target)

---

*Note: The 1,500 ms target for 1,000 parts depends on backend query efficiency. N+1 query degradation (Critical, see Thematic Synthesis) is a known risk to resolve for the pilot and before general production release.*
