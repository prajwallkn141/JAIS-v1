# JAIS v1 | Local Gmail review starter

A local, read-only n8n workflow that retrieves Gmail messages and creates a CSV for human review. The broader project explores how to identify job-application outcomes and assemble evidence using real Gmail screenshots.

**Current status: prototype.** The supplied workflow lists message IDs, retrieves message bodies, flags possible outcomes and generates a review CSV. A person must review every result. Labels were created manually in the case study; applying labels, scheduled monitoring and automatic Gmail screenshot/PDF production are not implemented in the supplied workflow. Opening messages in Gmail may mark them as read. Do not claim complete coverage without reconciling IDs and resolving extraction warnings.

## Start here

1. Read [the complete user manual](docs/USER-MANUAL.md) or [download the illustrated PDF](docs/pdf/JAIS-User-Manual.pdf).
2. For an explanation and interview practice, read [the project and interview guide](docs/INTERVIEW-GUIDE.md) or [the illustrated PDF](docs/pdf/JAIS-Interview-Guide.pdf).
3. Start Docker Desktop, run `docker compose up -d` from this directory, and open <http://localhost:5678>.
4. Configure your own Google Cloud project and Google OAuth client with `gmail.readonly`; import [the workflow](workflows/jais-read-only-review.json) and select your own credential in both Google HTTP nodes.
5. Run a bounded 10-message test before a full scan. Review and store the CSV privately.

## Important boundaries

- This repository contains no Gmail account, OAuth client secret, access token, mailbox data, or evidence screenshots. Never commit those or your n8n data volume.
- The default historical boundary is 1 October 2025, 00:00 Europe/Dublin. The guide explains how to select a different range or all dates. Spam and Trash are included in the list request; Gmail may permanently delete messages there.
- Gmail labels are not folders: adding a label need not remove the Inbox label. This prototype does not add any labels.
- Google Gmail API data scopes can be restricted and require verification for a multi-user production app. Each user setting up their own test project must add their own account as a test user. Testing-mode tokens for Gmail can expire after seven days.
- Gmail OAuth does not log a browser into Gmail. An API-generated rendering is not a screenshot of Gmail. The evidence process used browser screenshots and manual review, and is not provided as a one-click feature.
- n8n stores its own settings, workflows and credentials in its persistent volume; this is not a separate job-application database. Gmail is the source of original messages.

## Components

- `compose.yaml` - local n8n container, loopback-only port and persistent volume.
- `workflows/jais-read-only-review.json` - sanitized, inactive n8n workflow; attach your own credential after import.
- `docs/` - usage and interview guides plus non-sensitive instructional screenshots.

See [n8n's Docker documentation](https://docs.n8n.io/hosting/installation/docker/) and [Google's OAuth documentation](https://developers.google.com/identity/protocols/oauth2). Product screens can change.
