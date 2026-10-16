# Competitive Analysis & Journey Map (Module 2)

## Responses
- **Role, who are you solving for? (the specific user segment or profile):** An engineer who verifies parts for equipment reviews and is under delivery pressure with no sanctioned way to pull parts in bulk.
- **Goal, what is this user ultimately trying to achieve?:** Complete a BOM-wide equipment review on schedule without manually looking up each part.
- **Friction, the main barrier (moment of misery) stopping them from succeeding:** With no official bulk endpoint, hundreds of sequential lookups aren’t realistic. The engineer scrapes the database or passes an unencrypted CSV dump over an insecure channel to meet the deadline. They know this bypasses security protocols, and it leaves the company with no visibility into where the data goes.
- **External tools, the outside platforms or tools the user is forced to use:** Custom script wrappers or sequential GET requests (from the research)
Direct database scraping (from the research)
Unencrypted CSV dumps passed over insecure channels (from the research)
A spreadsheet or local file to cross-reference the BOM (assumption)
- **The process, the 3 to 5 manual steps the user takes to get the job done:** Receive the BOM for the equipment review and see that it has hundreds of parts to verify.
Try the sanctioned route first, writing a script wrapper to loop single-part GET requests.
The loop trips the rate limiters and fails partway through. With no native way to resume, they must restart or write retry logic.
Abandon the API and scrape the database directly to get the data in one pass.
Export the results as a CSV dump.
Share the CSV with teammates over an insecure channel, unencrypted.
Cross-reference the BOM against the dump manually and complete the review.
- **Core frustration, the exact moment the process feels most “broken”:** Wasted effort up front: Steps 2 and 3 are spent on a loop that fails, so time goes to a route that was never going to work.
Fragile and not repeatable: Scraped data is a one-off snapshot. It goes stale and can’t be refreshed without repeating the process, which echoes the stale-data problem in our bug reports.
Manual reconciliation: The engineer still has to cross-reference by hand, which is the manual burden the persona wanted to avoid.
Wrapper maintenance: Every engineer rebuilds this, so effort is duplicated across teams.
- **The evidence, a specific quote or behavior from the research that proves this:** The engineer isn’t careless. The workaround is rational under deadline pressure, which makes it a product failure and not a user failure. A secure, built-in batch endpoint would remove the reason for the hack.
- **Your journey map, a shareable link, or the map file you committed (e.g. journey-map.html):** https://github.com/mikefederman/pm-final-project/blob/main/02-discovery/future-state-journey-map.md
