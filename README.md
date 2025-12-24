# 🔍 Automated Domain Monitor

An easy-to-use, automated domain monitoring tool using GitHub Actions. This repository tracks domains that frequently change, keeping you informed about the latest updates.

## 📋 Overview

Many websites regularly change their domains (e.g., from `.com` to `.org` to `.net`). This tool allows you to:
- ✅ Monitor multiple domains simultaneously
- ✅ Automatically detect redirects and domain changes
- ✅ Store a complete history of all domain changes for each monitor
- ✅ Run checks every 6 hours via GitHub Actions
- ✅ Automatically commit changes back to the repository

## 🚀 Features

- **Multi-Domain Support**: Monitor as many domains as you want, each in its own directory.
- **Automatic Checks**: Runs every 6 hours via a GitHub Actions cron job.
- **Redirect Detection**: Follows HTTP redirects to find the final, effective URL.
- **Versioned History**: Stores all domain changes with timestamps in a `history.txt` file for each monitor.
- **Easy Access**: The current domain for any monitor is always available in a simple `domain.txt` file.
- **Automatic Commits**: All detected changes are automatically committed and pushed.

## 📂 File Structure

The repository is organized into individual monitors. Each subdirectory inside `monitors/` represents a separate domain being tracked.

```
.
├── .github/
│   └── workflows/
│       └── check_domains.yml    # GitHub Actions Workflow for all monitors
├── monitors/
│   ├── megakino/                # Example monitor directory
│   │   ├── domain.txt           # Contains the current domain (e.g., megakino.lol)
│   │   └── history.txt          # Logs all historical changes
│   └── another-service/         # Add another monitor just by creating a new folder
│       ├── domain.txt           # current-domain.com
│       └── history.txt          # ...
└── README.md                    # This file
```

## ⚙️ How It Works

1.  **Workflow Trigger**: The GitHub Actions workflow runs on a schedule (every 6 hours) or can be started manually.
2.  **Iterate Monitors**: The script loops through each subdirectory in the `monitors/` directory.
3.  **Read Domain**: For each monitor, it reads the current domain from its `domain.txt` file.
4.  **Check for Redirects**: An HTTP request follows all redirects to determine the final URL.
5.  **Compare & Update**: If the final domain is different from the stored one:
    - The `domain.txt` file is updated with the new domain.
    - A new entry with a timestamp is added to the `history.txt` file.
6.  **Commit Changes**: If any monitor was updated, all changes are bundled into a single commit and pushed to the repository.

## 🛠️ How to Add a New Domain to Monitor

Adding a new domain is as simple as creating a new folder and a file.

1.  **Create a Directory**:
    Create a new subdirectory inside the `monitors/` folder. The name of the directory will be the name of your monitor (e.g., `my-service`).
    ```bash
    mkdir monitors/my-service
    ```

2.  **Create the Domain File**:
    Inside your new directory, create a file named `domain.txt` that contains the initial domain you want to track.
    ```bash
    echo "example.com" > monitors/my-service/domain.txt
    ```

That's it! The GitHub Action will automatically pick up the new monitor on its next run. An empty `history.txt` will be created and populated automatically on the first change.

## 💡 Accessing the Current Domain

You can easily fetch the current domain for any monitor programmatically using GitHub's raw content URL. This is useful for integrating the latest domain into your own scripts or applications.

The URL format is:
`https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPOSITORY/main/monitors/MONITOR_NAME/domain.txt`

**Example**:
To get the current domain for the `megakino` monitor, you could use `curl`:
```bash
curl https://raw.githubusercontent.com/your-username/new-domain-check/main/monitors/megakino/domain.txt
```

## 📊 Example Output

### `monitors/megakino/domain.txt`
```
megakino.lol
```

### `monitors/megakino/history.txt`
```
2025-12-24 13:26:59 UTC | megakino.do → megakino.lol
2025-12-26 12:36:45 UTC | megakino.com → megakino.do
```

## 🤝 Contributing

Contributions are welcome! Feel free to create issues or pull requests for bug fixes, new features, or documentation improvements.

## 📝 License

This project is licensed under the MIT License.
