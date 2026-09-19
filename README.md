\# Google Drive Bidirectional Sync for Obsidian



A powerful Obsidian plugin that keeps your vault in sync across devices using Google Drive. It supports two‑way sync, conflict resolution, and works on both desktop (Windows, macOS, Linux) and mobile (Android, iOS).



\[!\[GitHub release](https://img.shields.io/github/v/release/your-username/gdrive-bidirectional-sync)](https://github.com/your-username/gdrive-bidirectional-sync/releases)

\[!\[License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)



\---



\## Features



\- \*\*Two‑way sync\*\* between your local vault and Google Drive.

\- \*\*Automatic folder creation\*\* – mirrors your vault structure in Drive.

\- \*\*Conflict resolution\*\* – choose between `newer`, `local`, `remote`, or `ask` (creates a conflict file).

\- \*\*Smart timestamp \& hash comparison\*\* – reduces unnecessary conflicts by using content hashes and local edit timestamps stored on Drive.

\- \*\*Deletion markers\*\* – deleted files are propagated safely across devices.

\- \*\*Manual actions\*\* – `Sync now`, `Force upload all`, `Force download all`.

\- \*\*Status bar \& ribbon icon\*\* – quick access to sync status.

\- \*\*Cross‑platform\*\* – works on Windows, macOS, Linux, Android, and iOS.

\- \*\*Exclude patterns\*\* – skip folders like `.obsidian` or `.trash`.



\---



\## Installation



\### From Obsidian Community Plugins (Recommended)

1\. Open Obsidian → \*\*Settings\*\* → \*\*Community Plugins\*\* → \*\*Browse\*\*.

2\. Search for \*\*Google Drive Bidirectional Sync\*\*.

3\. Click \*\*Install\*\*, then \*\*Enable\*\*.



\### Manual Installation

1\. Download the latest release from the \[Releases page](https://github.com/your-username/gdrive-bidirectional-sync/releases).

2\. Extract the ZIP into your vault's plugin folder:  

&#x20;  `<your-vault>/.obsidian/plugins/gdrive-bidirectional-sync/`

3\. Ensure the folder contains `main.js`, `manifest.json`, and `styles.css`.

4\. Reload Obsidian and enable the plugin in \*\*Settings → Community Plugins\*\*.



\---



\## Setup Guide



\### 1. Create Google Cloud OAuth Credentials



You need a Google Cloud project with the Drive API enabled and OAuth credentials.



1\. Go to the \[Google Cloud Console](https://console.cloud.google.com/).

2\. Create a new project (or select an existing one).

3\. Navigate to \*\*APIs \& Services → Library\*\*, search for \*\*Google Drive API\*\*, and \*\*Enable\*\* it.

4\. Go to \*\*APIs \& Services → OAuth consent screen\*\*.

&#x20;  - Choose \*\*External\*\* user type.

&#x20;  - Fill in the required fields (App name, User support email, Developer contact).

&#x20;  - \*\*Scopes\*\*: Add `https://www.googleapis.com/auth/drive.file` (this is a non‑sensitive scope that only allows access to files created by the app).

&#x20;  - \*\*Test users\*\*: Add your own Google account email.

5\. \*\*Publish the app to production\*\* (important – see below).

6\. Go to \*\*APIs \& Services → Credentials\*\* → \*\*Create Credentials\*\* → \*\*OAuth client ID\*\*.

&#x20;  - Application type: \*\*TVs and Limited Input devices\*\* (this enables the device flow, which works on mobile).

&#x20;  - Copy the \*\*Client ID\*\* and \*\*Client Secret\*\*.



> \*\*Why publish to production?\*\*  

> Google limits refresh tokens to 7 days when the OAuth consent screen is in \*\*Testing\*\* mode. Publishing to \*\*Production\*\* removes this limit, so you won't have to sign in every week.  

> \*\*Rest assured:\*\* publishing does \*\*not\*\* make your Drive public. Each user authenticates with their own Google account, and the `drive.file` scope ensures the app can only see files it creates. Your personal Drive remains private.



\### 2. Configure the Plugin



1\. Open Obsidian → \*\*Settings\*\* → \*\*Community Plugins\*\* → \*\*Google Drive Bidirectional Sync\*\*.

2\. Paste your \*\*Client ID\*\* and \*\*Client Secret\*\* into the respective fields.

3\. Click \*\*Sign in with Google\*\*.

4\. A modal will show a code and a URL. Open the URL in your browser, enter the code, and approve access.

5\. Once authorized, the plugin will create a folder in your Drive (named after your vault) and begin syncing.



\---



\## Usage



\- \*\*Sync now\*\*: Click the ribbon icon (refresh) or the status bar text, or use the command palette (`Ctrl/Cmd + P` → `Sync now`).

\- \*\*Auto‑sync\*\*: Set an interval in the settings (0 to disable).

\- \*\*Sync on startup\*\*: Automatically sync a few seconds after Obsidian launches.

\- \*\*Force upload/download\*\*: Use the settings tab or command palette to overwrite one side with the other. \*\*Use with caution\*\* – back up your vault first.



\### Settings



| Setting | Description |

|---------|-------------|

| \*\*Client ID\*\* | OAuth 2.0 Client ID from Google Cloud. |

| \*\*Client Secret\*\* | OAuth 2.0 Client Secret. |

| \*\*Auto‑sync interval\*\* | Minutes between automatic syncs (0 = disabled). |

| \*\*Sync on startup\*\* | Run a sync shortly after Obsidian starts. |

| \*\*Conflict strategy\*\* | `newer` (keep latest edit), `local` (always keep local), `remote` (always keep remote), `ask` (save both versions as a conflict file). |

| \*\*Exclude patterns\*\* | Comma‑separated paths to skip (default: `.obsidian,.trash`). |



\### Commands



\- `GDrive Sync: Sync now`

\- `GDrive Sync: Show sync status`

\- `GDrive Sync: Force upload all files to Google Drive`

\- `GDrive Sync: Force download all files from Google Drive`



\---



\## How Conflict Resolution Works



The plugin stores metadata about each file in `.obsidian/gdrive-sync-record.json` and on Google Drive (via `appProperties`).



\- \*\*Local edit timestamp\*\* (`lastLocalMtime`) is stored on Drive when a file is uploaded.

\- \*\*Content hash\*\* (`lastLocalHash`) is also stored, allowing the plugin to detect identical content even if timestamps differ.

\- When both sides have changed, the chosen conflict strategy is applied. For `newer`, the plugin compares the local edit time with the remote uploader's timestamp.



This approach minimises false conflicts caused by clock differences between devices.



\---



\## Privacy \& Security



\- The plugin uses the `drive.file` scope, which only permits access to files \*\*created by the app\*\*. It cannot read, modify, or delete any other files in your Google Drive.

\- OAuth credentials and tokens are stored locally in your vault's plugin data (`data.json`). Keep this file secure.

\- No data is sent to any third‑party servers – all communication is directly between your device and Google's API.



\---



\## Troubleshooting



\*\*I have to sign in every few days.\*\*  

Your OAuth consent screen is likely still in \*\*Testing\*\* mode. Publish it to \*\*Production\*\* in the Google Cloud Console to get long‑lived refresh tokens.



\*\*Sync fails with authentication errors.\*\*  

Go to the plugin settings and click \*\*Re‑authenticate\*\* to obtain a new token.



\*\*A previous sync crashed.\*\*  

The plugin resets the `syncInProgress` flag on startup and rebuilds state on the next sync.



\*\*Large divergence between devices.\*\*  

Use \*\*Force upload all\*\* or \*\*Force download all\*\* to realign, but always back up your vault first.



\*\*Conflict files keep appearing.\*\*  

Check that your devices' clocks are reasonably in sync. The plugin uses timestamps and content hashes to reduce conflicts, but large clock skew can still cause issues.



\---



\## Development



This plugin is written in TypeScript and bundled with esbuild.



```bash

\# Clone the repository

git clone https://github.com/your-username/gdrive-bidirectional-sync.git

cd gdrive-bidirectional-sync



\# Install dependencies

npm install



\# Build for production

npm run build



\# Development mode (watch)

npm run dev

