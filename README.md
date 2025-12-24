# 🔍 New Domain Check

Automated domain monitoring with GitHub Actions. This repository tracks domains that frequently change their top-level domain (TLD), keeping you informed about changes.

## 📋 Overview

Many websites regularly change their domain endings (e.g., from `.do` to `.lol` to `.com`). This tool:
- ✅ Automatically checks the current domain
- ✅ Detects redirects and domain changes
- ✅ Stores the history of all domain changes
- ✅ Runs every 5 minutes via GitHub Actions
- ✅ Automatically commits changes to the repository

## 🚀 Features

- **Automatic Checks**: Runs every 5 minutes via GitHub Actions cron job
- **Redirect Detection**: Follows HTTP redirects and extracts the final URL
- **Domain History**: Stores all domain changes with timestamps in `megakino-domain-history.txt`
- **Current Domain**: The currently active domain is stored in `current-megakino-domain.txt`
- **Automatic Commits**: Changes are automatically pushed to the repository

## 📂 File Structure

```
.
├── .github/
│   └── workflows/
│       └── check-megakino-domain.yml    # GitHub Actions Workflow
├── current-megakino-domain.txt          # Current domain
├── megakino-domain-history.txt          # History of all changes
└── README.md                            # This file
```

## ⚙️ How It Works

1. **Workflow Trigger**: The GitHub Actions workflow runs every 5 minutes (`cron: '*/5 * * * *'`)
2. **Read Domain**: The current domain is read from `current-megakino-domain.txt`
3. **Redirect Check**: An HTTP request follows all redirects to the final URL
4. **Comparison**: The final domain is compared with the stored domain
5. **On Change**:
   - The new domain is saved to `current-megakino-domain.txt`
   - An entry with timestamp is added to the history
   - Changes are automatically committed and pushed

## 🛠️ Customization for Your Own Domains

### 1. Fork Repository

Fork this repository to your own GitHub account.

### 2. Change Domain

Edit `current-megakino-domain.txt` and enter your domain to monitor:

```bash
echo "your-domain.com" > current-megakino-domain.txt
```

### 3. Customize Workflow (Optional)

Edit `.github/workflows/check-megakino-domain.yml` and adjust the following values:

- **Workflow Name**: Line 1
- **Cron Schedule**: Line 4 (default: every 5 minutes)
- **File Names**: If you want to use different file names

### 4. Set Permissions

Make sure the workflow has write permissions:

1. Go to **Settings** → **Actions** → **General**
2. Under **Workflow permissions** select **Read and write permissions**
3. Enable **Allow GitHub Actions to create and approve pull requests**

## 📊 Example Output

### current-megakino-domain.txt
```
megakino.lol
```

### megakino-domain-history.txt
```
2025-12-24T13:45:23 UTC | megakino.do | megakino.lol
2025-12-20T09:12:45 UTC | megakino.com | megakino.do
2025-12-15T14:30:11 UTC | megakino.net | megakino.com
```

## 🔧 Technical Details

### GitHub Actions Workflow

The workflow uses:
- **Ubuntu Latest** as runner
- **Bash scripting** for domain checks
- **curl** with `--location` flag to follow redirects
- **Git automation** for automatic commits

### Redirect Tracking

```bash
curl --silent --location --output /dev/null --write-out "%{url_effective}" "$DOMAIN"
```

This command:
- Follows all HTTP redirects (`--location`)
- Outputs the final URL (`--write-out`)
- Discards the response body (`--output /dev/null`)

## 🤝 Contributing

Contributions are welcome! Feel free to create issues or pull requests for:
- Bug fixes
- New features
- Documentation improvements
- Additional domain checks

## 📝 License

This project is licensed under the MIT License.

## ⚠️ Notice

This tool is intended for monitoring purposes. Please respect the robots.txt and terms of service of the monitored websites.

---

**Created with ❤️ and GitHub Actions**
