Here is a complete, GitHub‑ready `README.md` that includes **all** the details we discussed: per‑user OAuth setup, the production‑publishing step, automatic `data.json` creation, privacy guarantees, usage, troubleshooting, and developer notes.

# Google Drive Bidirectional Sync for Obsidian

A powerful Obsidian plugin that keeps your vault in sync across devices using **your own** Google Drive. It supports two‑way sync, conflict resolution, and works on desktop (Windows, macOS, Linux) and mobile (Android, iOS).

[![GitHub release](https://img.shields.io/github/v/release/your-username/gdrive-bidirectional-sync)](https://github.com/your-username/gdrive-bidirectional-sync/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## Features

- **Two‑way sync** between your local vault and Google Drive.
- **Automatic folder creation** – mirrors your vault structure in Drive.
- **Conflict resolution** – choose between `newer`, `local`, `remote`, or `ask` (creates a conflict file).
- **Smart timestamp & hash comparison** – reduces unnecessary conflicts by using content hashes and local edit timestamps stored on Drive.
- **Deletion markers** – deleted files are propagated safely across devices.
- **Manual actions** – `Sync now`, `Force upload all`, `Force download all`.
- **Status bar & ribbon icon** – quick access to sync status.
- **Cross‑platform** – works on Windows, macOS, Linux, Android, and iOS.
- **Exclude patterns** – skip folders like `.obsidian` or `.trash`.

---

## Installation

### From Obsidian Community Plugins (Recommended)
1. Open Obsidian → **Settings** → **Community Plugins** → **Browse**.
2. Search for **Google Drive Bidirectional Sync**.
3. Click **Install**, then **Enable**.

### Manual Installation
1. Download the latest release from the [Releases page](https://github.com/your-username/gdrive-bidirectional-sync/releases).
2. Extract the ZIP into your vault's plugin folder:  
   `<your-vault>/.obsidian/plugins/gdrive-bidirectional-sync/`
3. Ensure the folder contains `main.js`, `manifest.json`, and `styles.css`.
4. Reload Obsidian and enable the plugin in **Settings → Community Plugins**.

---

## Setup Guide

### 1. Create Your Own Google Cloud OAuth Credentials

> **Why do I need to do this?**  
> This plugin runs entirely on your device. To keep your data private and avoid sharing API quotas, **each user creates their own OAuth credentials**. You only do this once, and it takes about 5 minutes. Your credentials are stored locally in your vault and are never shared with anyone.

1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. **Create a new project** (or select an existing one).  
   *Example name: `Obsidian GDrive Sync`.*
3. **Enable the Google Drive API**:
   - Navigate to **APIs & Services → Library**.
   - Search for **Google Drive API**.
   - Click **Enable**.
4. **Configure the OAuth consent screen**:
   - Go to **APIs & Services → OAuth consent screen**.
   - Choose **External** user type (unless you have a Google Workspace account, then you can choose Internal).
   - Fill in the required fields:
     - **App name**: anything you like (e.g., `Obsidian GDrive Sync`).
     - **User support email**: your email.
     - **Developer contact information**: your email.
   - Click **Save and Continue**.
   - **Scopes**: Click **Add or Remove Scopes**.
     - Add `https://www.googleapis.com/auth/drive.file`  
       *(This scope only allows the app to see and manage files it creates. It cannot access any other files in your Drive.)*
     - Click **Update** and then **Save and Continue**.
   - **Test users**: Add your own Google account email address.
   - Click **Save and Continue**.
5. **Publish the app to production** (important – see below).
   - On the OAuth consent screen page, under **Publishing status**, click **PUBLISH APP**.
   - Confirm. The status will change to **In production**.
   - *Why?* Google limits refresh tokens to **7 days** when the app is in **Testing** mode. Publishing to production removes this limit, so you won't have to sign in every week.
   - **Rest assured:** publishing does **not** make your Drive public. Each user authenticates with their own Google account, and the `drive.file` scope ensures the app can only see files it creates. Your personal Drive remains private.
6. **Create OAuth Client ID**:
   - Go to **APIs & Services → Credentials**.
   - Click **Create Credentials → OAuth client ID**.
   - Application type: **TVs and Limited Input devices**  
     *(This enables the device flow, which works on mobile and desktop.)*
   - Give it a name (e.g., `Obsidian Sync`).
   - Click **Create**.
   - Copy the **Client ID** and **Client Secret**. You'll need them in the next step.

### 2. Configure the Plugin

1. Open Obsidian → **Settings** → **Community Plugins** → **Google Drive Bidirectional Sync**.
2. Paste your **Client ID** and **Client Secret** into the respective fields.
3. Click **Sign in with Google**.
4. A modal will show a code and a URL. Open the URL in your browser, enter the code, and approve access.
5. Once authorized, the plugin will create a folder in your Drive (named after your vault) and begin syncing.

> **Note:** The plugin automatically creates its own `data.json` file in the plugin folder to store your settings and tokens. You never need to create or edit this file manually. It is not included in the repository and should never be shared.

---

## Usage

- **Sync now**: Click the ribbon icon (refresh) or the status bar text, or use the command palette (`Ctrl/Cmd + P` → `Sync now`).
- **Auto‑sync**: Set an interval in the settings (0 to disable).
- **Sync on startup**: Automatically sync a few seconds after Obsidian launches.
- **Force upload/download**: Use the settings tab or command palette to overwrite one side with the other. **Use with caution** – back up your vault first.

### Settings

| Setting | Description |
|---------|-------------|
| **Client ID** | OAuth 2.0 Client ID from Google Cloud. |
| **Client Secret** | OAuth 2.0 Client Secret. |
| **Auto‑sync interval** | Minutes between automatic syncs (0 = disabled). |
| **Sync on startup** | Run a sync shortly after Obsidian starts. |
| **Conflict strategy** | `newer` (keep latest edit), `local` (always keep local), `remote` (always keep remote), `ask` (save both versions as a conflict file). |
| **Exclude patterns** | Comma‑separated paths to skip (default: `.obsidian,.trash`). |

### Commands

- `GDrive Sync: Sync now`
- `GDrive Sync: Show sync status`
- `GDrive Sync: Force upload all files to Google Drive`
- `GDrive Sync: Force download all files from Google Drive`

---

## How Conflict Resolution Works

The plugin stores metadata about each file in `.obsidian/gdrive-sync-record.json` and on Google Drive (via `appProperties`).

- **Local edit timestamp** (`lastLocalMtime`) is stored on Drive when a file is uploaded.
- **Content hash** (`lastLocalHash`) is also stored, allowing the plugin to detect identical content even if timestamps differ.
- When both sides have changed, the chosen conflict strategy is applied. For `newer`, the plugin compares the local edit time with the remote uploader's timestamp.

This approach minimises false conflicts caused by clock differences between devices.

---

## Privacy & Security

- The plugin uses the `drive.file` scope, which only permits access to files **created by the app**. It cannot read, modify, or delete any other files in your Google Drive.
- **Each user authenticates with their own Google account.** Files are stored in **their own** Google Drive and count against **their own** storage quota. Your Drive is never touched.
- OAuth credentials and tokens are stored locally in your vault's plugin data (`data.json`). Keep this file secure and never commit it to Git.
- No data is sent to any third‑party servers – all communication is directly between your device and Google's API.

---

## Troubleshooting

**I have to sign in every few days.**  
Your OAuth consent screen is likely still in **Testing** mode. Publish it to **Production** in the Google Cloud Console to get long‑lived refresh tokens.

**Sync fails with authentication errors.**  
Go to the plugin settings and click **Re‑authenticate** to obtain a new token.

**A previous sync crashed.**  
The plugin resets the `syncInProgress` flag on startup and rebuilds state on the next sync.

**Large divergence between devices.**  
Use **Force upload all** or **Force download all** to realign, but always back up your vault first.

**Conflict files keep appearing.**  
Check that your devices' clocks are reasonably in sync. The plugin uses timestamps and content hashes to reduce conflicts, but large clock skew can still cause issues.

**I accidentally deleted `data.json`.**  
No problem – the plugin will recreate it with default settings the next time you change a setting or sign in.

---

## For Developers

This plugin is written in TypeScript and bundled with esbuild.

```bash
# Clone the repository
git clone https://github.com/your-username/gdrive-bidirectional-sync.git
cd gdrive-bidirectional-sync

# Install dependencies
npm install

# Build for production
npm run build

# Development mode (watch)
npm run dev
```

The bundled output is `main.js`.

### Important: Do not commit `data.json`

`data.json` contains sensitive OAuth credentials and tokens. It is generated at runtime and **must not** be pushed to GitHub. Ensure your `.gitignore` includes:

```gitignore
data.json
node_modules/
.DS_Store
```

If you have already committed `data.json`, revoke your credentials in the Google Cloud Console, remove the file from Git history, and create new credentials.

---

## Contributing

Pull requests are welcome! Please open an issue first to discuss any major changes.

1. Fork the repository.
2. Create a feature branch.
3. Commit your changes.
4. Push to the branch.
5. Open a Pull Request.

---

## License

This project is licensed under the MIT License – see the [LICENSE](LICENSE) file for details.

---

## Support

- Open an issue on [GitHub](https://github.com/your-username/gdrive-bidirectional-sync/issues).
- If you find this plugin useful, consider buying me a coffee ☕ (optional).

---

*Not affiliated with Google or Obsidian.*

### What to do next

1. **Replace placeholders**: Change `your-username` to your actual GitHub username in the badges and links.
2. **Add a `LICENSE` file**: Create a `LICENSE` file with the MIT license text (or your preferred license).
3. **Add screenshots**: Consider adding a few screenshots of the settings and the sign‑in modal to make the README more appealing. Place them in a `screenshots/` folder and reference them like `![Settings](screenshots/settings.png)`.
4. **Add a `.gitignore`**: Ensure `data.json` and `node_modules/` are ignored.
5. **Push to GitHub**: Commit and push the `README.md` along with your plugin files (`main.js`, `manifest.json`, `styles.css`).

This README clearly explains the per‑user OAuth setup, highlights the production‑publishing step, and reassures users about privacy. It should make your plugin look professional and help others get started quickly.