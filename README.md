# Gmail IMAP Timestamp Fixer

> **Post-Migration Date Recovery Tool:** Fixes incorrect email timestamps in Gmail after IMAP migrations. Automatically extracts the original `Date:` header from MIME bodies, recreates the emails with correct historical `INTERNALDATE` metadata, and purges the incorrect duplicates from Gmail.

---

<p align="center">
  <a href="https://www.buymeacoffee.com/agent47.scripts" target="_blank" rel="noopener noreferrer">
    <img src="https://img.shields.io/badge/Buy%20Me%20A%20Coffee-Donate-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black" alt="Buy Me A Coffee">
  </a>
</p>

---

## 🛑 The Problem

When migrating emails to Gmail via standard IMAP tools or third-party importers, network drops, server timeouts, or improper `APPEND` flags often cause Gmail to assign the **current import timestamp** (`INTERNALDATE`) to historical messages. 

As a result, emails that are 10 or 20 years old all appear in Gmail's web interface as if they were received *today* at the exact time of the migration. Because IMAP's `INTERNALDATE` is strictly read-only after creation, standard clients cannot update these timestamps in-place.

## 🚀 The Solution

This tool automates the in-place repair process:
1. Connects directly to your target Gmail / Google Workspace account over IMAP.
2. Iterates through all folders (or a single folder) and parses each email's MIME body to extract its true original `Date:` header.
3. Re-appends the email to Gmail using an explicitly formatted RFC3501 `INTERNALDATE` string.
4. Safely removes the original email with the incorrect import timestamp.
5. Preserves all message flags, attachments, formatting, and folder structures.

---

## 🔑 Prerequisites & Account Setup

Modern email providers require **App-Specific Passwords** for automated script access:

1. Go to your [Google Account Settings](https://myaccount.google.com/security) (**2-Step Verification** must be enabled).
2. Direct link: [Google App Passwords](https://myaccount.google.com/apppasswords).
3. Create a new app password (e.g. named "Timestamp Fixer").
4. Copy the **16-digit code** (without spaces or hyphens, e.g. `abcd1234efgh5678`).

---

## ⚙️ Usage & Execution

Set your Target Account environment variables before executing the script. Choose the command block corresponding to your operating system:

### Option A: Linux / macOS (Terminal)

```bash
# Set your Gmail / Google Workspace credentials
export TGT_USER="your-email@gmail.com"
export TGT_PASS="abcd1234efgh5678"

# Run the fixer script
python3 gmail_timestamp_fixer.py
```

### Option B: Windows CMD (Command Prompt)

```cmd
:: Set your Gmail / Google Workspace credentials
set TGT_USER=your-email@gmail.com
set TGT_PASS=abcd1234efgh5678

:: Run the fixer script
python gmail_timestamp_fixer.py
```

### Option C: Windows PowerShell

```powershell
# Set your Gmail / Google Workspace credentials
$env:TGT_USER="your-email@gmail.com"
$env:TGT_PASS="abcd1234efgh5678"

# Run the fixer script
python gmail_timestamp_fixer.py
```

---

## 🛡️ Reliability & Safety Features

- **Auto-Reconnect & Throttling:** Built-in SSL connection retry loops and throttling delays (`time.sleep(0.3)`) prevent Google rate-limits and socket resets during large repair runs.
- **mUTF-7 Safe:** Handles localized folder names with umlauts or non-ASCII characters (`ä`, `ö`, `ü`, `ß`).
- **Duplicate Prevention:** Uses `Message-ID` verification or MD5 fallback hashing to prevent creating duplicate emails during repair interruptions.
- **System Folders Ignored:** Standard system folders (`[Gmail]`, `Trash`, `Spam`, etc.) are automatically skipped to avoid corrupting special system structures.

---

## 📜 License
Licensed under the [MIT License](LICENSE). Free to use, adapt, and share.