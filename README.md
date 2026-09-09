# ⚡ PowerShell 80/20 — Dev Cheat Sheet

> **Only 40 PowerShell commands that cover 90% of daily development work.**

**English · Beginner Friendly · Free to Share**

<div align="center">

![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-5391FE?logo=powershell\&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green.svg)
![Commands](https://img.shields.io/badge/Commands-40-orange)
![Beginner Friendly](https://img.shields.io/badge/Beginner-Friendly-brightgreen)

</div>

---

## 🤔 What Is This?

**PowerShell 80/20** is a practical cheat sheet based on the **80/20 rule**.

Instead of trying to memorize hundreds of PowerShell commands, this guide focuses on the **40 commands and patterns you'll actually use in everyday development**.

> **Don't memorize everything. Understand the patterns and look up what you need.**

---

## 🎯 Who Is This For?

* 👨‍💻 Junior / Mid-level Software Engineers
* ⚙️ DevOps & SRE beginners
* 🧑‍🎓 Students learning development
* 🖥️ Developers switching from CMD / Bash
* 👥 Teams looking for a quick PowerShell reference

---

## 📂 Table of Contents

| #  | Section             | What It Covers                          |
| -- | ------------------- | --------------------------------------- |
| 1  | 📁 Navigation       | Moving around folders                   |
| 2  | 🔍 Search & Read    | Searching logs and reading files        |
| 3  | 🔗 Pipeline         | Filter, select and sort data            |
| 4  | ⚙️ Processes        | Manage processes and fix port conflicts |
| 5  | 🌐 Networking       | Test APIs and connectivity              |
| 6  | 📊 Data             | Work with JSON and CSV                  |
| 7  | 📜 Scripting        | Write quick scripts                     |
| 8  | 💡 Pro Tips         | Shortcuts and useful tricks             |
| 9  | 🧠 Command Patterns | Understand PowerShell naming            |
| 10 | 📋 Quick Reference  | Everything at a glance                  |

> 📖 **Full command reference:** [`powershell-cheatsheet`](./powershell-cheatsheet.md)

---

# ⚡ Quick Preview

## 🔌 Port Conflict Fix

One of the most common problems during development:

**"Port 3000 is already in use!"**

```powershell
# Find the process using port 3000
$port = Get-NetTCPConnection -LocalPort 3000

# Stop the process
Stop-Process -Id $port.OwningProcess -Force

Write-Host "Port free!" -ForegroundColor Green
```

---

## 🌐 API Test — One Line

Get information from an API:

```powershell
# Get user information from GitHub
$me = Invoke-RestMethod "https://api.github.com/users/octocat"

$me.name
# → The Octocat
```

Short alias:

```powershell
$me = irm "https://api.github.com/users/octocat"
```

---

## 🔍 Log Search — Like `grep`

Search for `"error"` inside a log file:

```powershell
Select-String "error" -Path "C:\logs\app.log" -Context 2
```

Short alias:

```powershell
sls "error" -Path "C:\logs\app.log" -Context 2
```

---

## 🔗 The Golden Pipeline

Filter processes using more than 50 CPU and sort them by CPU usage:

```powershell
Get-Process |
    Where-Object { $_.CPU -gt 50 } |
    Select-Object Name, CPU |
    Sort-Object CPU -Descending
```

Short aliases:

```powershell
Get-Process |
    ? { $_.CPU -gt 50 } |
    select Name, CPU |
    sort CPU -desc
```

> 💡 **Pipeline = Get → Filter → Select → Sort**

---

# 📊 Project Stats

| Metric                     | Value                   |
| -------------------------- | ----------------------- |
| 📦 Total Commands          | **40**                  |
| 🎯 Daily Work Coverage     | **~90%**                |
| ⏱️ Suggested Learning Time | **2–3 days**            |
| 💻 Platforms               | Windows · macOS · Linux |
| 🟦 PowerShell Version      | **7+**                  |
| 📜 License                 | **MIT**                 |

---

# 🚀 Getting Started

## Step 1 — Install PowerShell

### Windows

```powershell
winget install Microsoft.PowerShell
```

### macOS

```bash
brew install --cask powershell
```

### Linux

```bash
sudo snap install powershell --classic
```

---

## Step 2 — Open the Cheat Sheet

1. Download [`powershell-80-20.md`](./powershell-80-20.md)
2. Open it in **VS Code**
3. Press `Ctrl + Shift + V` to open Markdown Preview
4. Bookmark it for quick reference

---

## Step 3 — Start Using It

You **don't need to memorize everything**.

Use this workflow:

```text
Need something
      ↓
Look it up
      ↓
Try the command
      ↓
Understand what it does
      ↓
Use it again
      ↓
Eventually remember it
```

### Remember:

> **Get-Help + Search = Fast Solution**

---

# 🧠 The PowerShell Pattern

This is one of the most useful things to understand.

PowerShell commands usually follow a:

```text
VERB-NOUN
```

pattern.

| Pattern     | Meaning          | Example          |
| ----------- | ---------------- | ---------------- |
| `Get-*`     | Read / Retrieve  | `Get-Process`    |
| `Set-*`     | Update / Modify  | `Set-Content`    |
| `New-*`     | Create           | `New-Item`       |
| `Remove-*`  | Delete           | `Remove-Item`    |
| `Copy-*`    | Copy             | `Copy-Item`      |
| `Move-*`    | Move / Rename    | `Move-Item`      |
| `Test-*`    | Check / Validate | `Test-Path`      |
| `Start-*`   | Start            | `Start-Process`  |
| `Stop-*`    | Stop             | `Stop-Process`   |
| `Export-*`  | Export data      | `Export-Csv`     |
| `Import-*`  | Import data      | `Import-Csv`     |
| `Convert-*` | Convert data     | `ConvertTo-Json` |

### 💡 The Big Idea

If you understand the pattern:

```text
Get
Set
New
Remove
Copy
Move
Test
Start
Stop
Import
Export
Convert
```

you can often **guess or discover unfamiliar commands much faster**.

> 🧠 **Understand the pattern → Learn fewer commands → Do more work**

---

# 💡 Top 5 PowerShell Tips

| # | Tip           | What It Does                          |
| - | ------------- | ------------------------------------- |
| 1 | `Tab`         | Autocomplete commands and paths       |
| 2 | `Get-Help`    | Built-in help for commands            |
| 3 | `Get-Command` | Find commands by keyword              |
| 4 | `?` / `%`     | Shortcuts for filtering and looping   |
| 5 | `-WhatIf`     | Preview an action before executing it |

### Example

```powershell
Get-Help Get-Process
```

Find commands related to processes:

```powershell
Get-Command *Process*
```

Preview a potentially destructive command:

```powershell
Remove-Item .\test.txt -WhatIf
```

---

# 🤝 Contributing

Contributions are welcome!

You can help by:

* ✅ Adding a useful developer use case
* ✅ Adding a missing command
* ✅ Improving existing examples
* ✅ Fixing documentation
* ✅ Reporting errors
* ✅ Submitting a Pull Request

## Contribution Workflow

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/powershell-80-20.git

# Enter the project
cd powershell-80-20

# Create a new branch
git checkout -b feature/your-feature

# Make your changes
# Edit powershell-80-20.md

# Stage changes
git add .

# Commit
git commit -m "Add: new use case for XYZ"

# Push
git push origin feature/your-feature
```

Then open a **Pull Request** on GitHub.

---

# 📄 License

This project is licensed under the **MIT License**.

You are free to:

* Use it
* Share it
* Modify it
* Learn from it
* Include it in your own projects

See [`LICENSE`](./LICENSE) for the complete license text.

---

# 🙏 Acknowledgments

Thanks to:

* [PowerShell Official Documentation](https://learn.microsoft.com/powershell/)
* PowerShell community contributors
* Developers sharing knowledge through open-source projects
* Everyone contributing examples and improvements

---

# ⭐ Found This Helpful?

If this cheat sheet saves you time, consider supporting the project:

| Platform           | What You Can Do                           |
| ------------------ | ----------------------------------------- |
| 🐙 GitHub          | ⭐ Star + 🍴 Fork                          |
| 💼 LinkedIn        | Share it with developers                  |
| 𝕏 Twitter/X       | Share the project                         |
| 💬 Discord / Slack | Drop it in your team channel              |
| 🟠 Reddit          | Share in relevant programming communities |

---

<div align="center">

### ⚡ Learn less. Build more.

**PowerShell 80/20 — Your daily PowerShell reference.**

⭐ If this helped you, consider giving the repository a star!

</div>
