# Privacy Policy — Jira Sync

_Last updated: 2026-05-16_

This policy describes how the **Jira Sync — Pull Requests, Worklog & Story Points** Chrome extension (the "Extension") handles your data.

## Summary

- The Extension does **not** collect, transmit, sell, or share any personal data with the developer or with any third party.
- The only network destinations the Extension contacts are **Jira hosts you configure yourself** in Settings.
- All configuration is stored locally in your browser via `chrome.storage.sync`.
- There is no analytics, telemetry, advertising, tracking, or remote code execution.

## What data the Extension stores

The Extension stores the following items, **locally in your browser**, via the standard `chrome.storage.sync` API. If you have Chrome Sync enabled, your browser may synchronise this data across your own signed-in devices; the Extension itself does not transmit it anywhere else.

| Item | Source | Purpose |
|---|---|---|
| Jira base URL | You enter it in Settings | Target host for REST API calls |
| Jira Personal Access Token (PAT) | You enter it in Settings | Sent as `Authorization: Bearer <PAT>` to your Jira host |
| URL match patterns | You enter them in Settings | Tells the Extension which pages it should activate on |
| List of Jira field names to display and update | You enter them in Settings | Configures the popup form |
| Granted optional host permissions | Granted by you via Chrome's permission prompt | Allows REST calls to your Jira origin |

None of these items are transmitted to the developer or to any third party.

## What the Extension transmits, and where

The Extension makes HTTPS requests **only** to the Jira base URL you configured in Settings, using the Jira REST API v2 endpoints needed to:

- Read the configured issue (`GET /rest/api/2/issue/{key}?expand=names,renderedFields`).
- List Jira fields (`GET /rest/api/2/field`).
- Update issue fields (`PUT /rest/api/2/issue/{key}`).
- Add a worklog (`POST /rest/api/2/issue/{key}/worklog`).
- Add a comment (`POST /rest/api/2/issue/{key}/comment`).
- Move the issue between sprints when you request it (`POST /rest/agile/1.0/sprint/{id}/issue`).

These requests are made **only** when you take an action in the popup (load a ticket, click Submit, etc.). The Extension does not send requests in the background or on a schedule. The Extension does not contact the developer, an analytics provider, a CDN, a remote update server, or any host other than the Jira host you configured.

## What the Extension reads from web pages

When you click the toolbar icon, Chrome grants the Extension temporary access to the active tab via the `activeTab` permission. The Extension injects a content script that:

- Reads the tab's URL.
- Reads visible text from the page body, the page title, comment bodies, and the `href` attributes of links.
- Looks for substrings matching the Jira issue-key pattern `[A-Z][A-Z0-9]+-\d+` and for `/browse/<KEY>` links.

The extracted keys are returned to the popup, in-memory, so it can show you the issue. **None of the page content is transmitted anywhere.** The content script does not run unless you click the toolbar icon, and runs only on the active tab.

## Host permissions

The Extension declares `optional_host_permissions: ["*://*/*"]` in its manifest. This **does not** grant access to any site at install time. The pattern exists only so the Extension can call `chrome.permissions.request` for **the specific origin you entered as your Jira base URL**, the first time you save Settings. Chrome will prompt you to grant access to that one origin; you may decline, in which case Jira API calls will fail with a clear error message and no data will be sent.

You can revoke this grant at any time from `chrome://extensions` → Jira Sync → **Site access**.

## Authentication credentials

The Extension treats your Jira Personal Access Token as sensitive:

- It is stored only in `chrome.storage.sync`.
- It is sent only as an `Authorization: Bearer` header, only to the Jira base URL you configured, only over HTTPS (assuming your Jira host uses HTTPS, which the Extension recommends).
- It is never logged, written to the page DOM, displayed in tooltips, included in error reports, or sent to the developer.
- It is not transmitted to any third party.

To remove the token, open Settings and clear the field, or remove the Extension entirely (`chrome://extensions` → Remove).

## Data retention

All stored data lives in your browser. The Extension has no server-side component and no database. Removing the Extension deletes its `chrome.storage.sync` entries from your browser. Chrome Sync may retain a copy on Google's servers while it is enabled in your browser — see Google's own privacy disclosures for that mechanism; the Extension developer has no access to it.

## Children

The Extension is a developer tool not directed at children under 13 and does not knowingly process data from children.

## Changes to this policy

Material changes to this policy will be reflected by updating the **Last updated** date at the top of this file. The current version of this policy is always available at its canonical URL:

<https://github.com/ragnar-lothbrok/jira-sync/blob/main/PRIVACY.md>

## Contact

For questions about this policy or to report a concern, open an issue at <https://github.com/ragnar-lothbrok/jira-sync/issues>, or contact the maintainer listed in the Chrome Web Store listing.
