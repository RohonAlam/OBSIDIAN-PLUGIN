Here’s a clean, professional **root‑level `README.md`** for your GitHub repository that will host multiple Obsidian plugins. It introduces the repo, lists the plugins (with the GDrive one as the first), provides general installation instructions, and includes developer/contribution guidelines. You can copy this directly and adjust as you add more plugins.

# Obsidian Plugins by [Your Name]

A collection of community plugins for [Obsidian](https://obsidian.md/), the powerful knowledge base that works on local Markdown files.

This repository hosts the source code and releases for all my Obsidian plugins. Each plugin lives in its own subfolder and has its own detailed README, but this page gives you an overview and general installation instructions.

---

## 📦 Available Plugins

| Plugin | Description | Status | Links |
|--------|-------------|--------|-------|
| **Google Drive Bidirectional Sync** | Two‑way sync between your vault and your own Google Drive. Supports conflict resolution, deletion markers, and works on desktop & mobile. | ✅ Active | [Folder](./gdrive-bidirectional-sync) · [README](./gdrive-bidirectional-sync/README.md) · [Releases](../../releases?q=gdrive) |
| *Your next plugin* | *Short description here* | 🚧 Planned | – |

> **Note:** Each plugin has its own README with detailed setup, usage, and troubleshooting. Click the folder link to view it.

---

## 🚀 Installation

### From Obsidian Community Plugins (Recommended)
Once a plugin is accepted into the official community store, you can install it directly:
1. Open Obsidian → **Settings** → **Community Plugins** → **Browse**.
2. Search for the plugin name.
3. Click **Install**, then **Enable**.

### Manual Installation
If a plugin is not yet in the community store, or you want to install a specific release:
1. Download the latest release ZIP from the [Releases page](../../releases).
2. Extract the contents into your vault’s plugin folder:  
   `<your-vault>/.obsidian/plugins/<plugin-id>/`
3. Ensure the folder contains at least `main.js`, `manifest.json`, and `styles.css` (if applicable).
4. Reload Obsidian and enable the plugin in **Settings → Community Plugins**.

Each plugin’s README will have specific installation notes if needed.

---

## 🛠️ Repository Structure

```
obsidian-plugins/
├── gdrive-bidirectional-sync/     # Google Drive sync plugin
│   ├── main.js
│   ├── manifest.json
│   ├── styles.css
│   ├── README.md
│   └── ...
├── another-plugin/                # Future plugin
│   └── ...
├── .gitignore
├── LICENSE
└── README.md                      # This file
```

Each plugin folder is self‑contained. Build outputs (`main.js`) are committed for release convenience, but source files and build scripts are also included.

---

## 🔒 Security & Privacy

- **Never commit `data.json`** – it contains OAuth tokens and other sensitive settings. It is generated at runtime by Obsidian and should be kept private.
- All plugins are designed with privacy in mind. For example, the Google Drive plugin uses the `drive.file` scope, which only allows access to files the plugin itself creates. Your personal data is never shared with third parties.
- See each plugin’s README for specific privacy details.

---

## 🧑‍💻 Development

### Prerequisites
- [Node.js](https://nodejs.org/) (v16 or later)
- [npm](https://www.npmjs.com/) or [pnpm](https://pnpm.io/)
- [Obsidian](https://obsidian.md/) for testing

### General Workflow
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/obsidian-plugins.git
   cd obsidian-plugins
   ```
2. Navigate to the plugin you want to work on:
   ```bash
   cd gdrive-bidirectional-sync
   ```
3. Install dependencies:
   ```bash
   npm install
   ```
4. Build the plugin:
   ```bash
   npm run build
   ```
   Or run in watch mode during development:
   ```bash
   npm run dev
   ```
5. Copy the built `main.js`, `manifest.json`, and `styles.css` into your test vault’s plugin folder, or use a symlink.

Each plugin may have its own `package.json` and build configuration. Check the plugin’s README for specific instructions.

---

## 🤝 Contributing

Contributions are welcome! If you find a bug or have a feature request:
1. Open an issue in this repository. Please mention the plugin name in the title (e.g., `[GDrive Sync] Bug: ...`).
2. If you want to submit a fix, fork the repo, create a feature branch, and open a pull request.

Please make sure your code follows the existing style and passes any tests (if present).

---

## 📄 License

This repository and all plugins within are licensed under the **MIT License** unless otherwise stated in a plugin’s folder. See the [LICENSE](./LICENSE) file for details.

---

## 💬 Support

- For general questions, open a discussion or issue.
- For plugin‑specific issues, please use the issue tracker and tag the plugin name.
- If you find these plugins useful, consider starring ⭐ the repository or buying me a coffee ☕.

---

*Not affiliated with Obsidian.md or any of the services these plugins integrate with.*


### How to use this

1. **Replace placeholders**:
   - `[Your Name]` with your name or GitHub username.
   - `your-username` in the clone URL.
   - The second plugin row is just an example; remove it or replace it as you add more plugins.

2. **Adjust the repository structure** if your folder names differ (e.g., if you put each plugin in a separate repo instead of a monorepo). If you plan to use a single repo for all plugins, the structure shown is ideal.

3. **Add a top‑level `LICENSE`** file (MIT recommended) if you haven’t already.

4. **Update the plugin list** as you add new plugins. You can also add badges for each plugin (e.g., release version, license) if you like.

5. **Link to each plugin’s README** correctly. If the GDrive plugin folder is named `gdrive-bidirectional-sync`, the link `./gdrive-bidirectional-sync/README.md` will work.

This root README gives visitors a clear overview, makes installation easy, and sets expectations for privacy and contribution. It’s a solid foundation for your multi‑plugin repository.
