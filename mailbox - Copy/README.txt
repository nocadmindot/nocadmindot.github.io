# Mailbox performance-optimized build

## Files
- mailbox-optimized.html — original interface with external deferred scripts.
- mailbox-app.js — API client, mailbox UI, folders, list, thread, compose and interaction logic.
- mailbox-profile.js — profile drawer, profile editing and appearance preferences.

## Deployment
Upload all three files to the same directory/repository path. Keep mailbox-common.css,
mailbox-mobile.css, favicon.png, and notification.mp3 alongside the HTML as before.
If hosted in a subfolder, keep both JavaScript files in that same subfolder or adjust
their src paths.

The Google Apps Script Web App URL remains configured at the top of mailbox-app.js.
No Apps Script backend change is required for these client-side optimizations.

## Main optimizations
- In-flight request deduplication for read-only API operations.
- Two-minute in-memory thread cache, invalidated after thread mutations.
- Thread profile-photo lookup runs concurrently and no longer delays first thread paint.
- Delegated list-row event handlers replace repeated per-row event binding after renders.
- Folder cache TTL increased from 45 to 60 seconds; silent refresh remains enabled.
- External scripts use defer and retain their original execution order.

Client-side changes cannot eliminate Apps Script execution or spreadsheet-access latency.
If delays remain, optimize the Apps Script mailbox and getThread actions using batched reads,
indexed lookups, and fewer full-sheet scans.
