# Powershell-CheatSheet
| PowerShell Cheat Sheet – Only 40 commands for 90% daily dev work | 

<div align="center">

# ⚡ PowerShell 80/20 – Dev Cheat Sheet

**Only 40 commands that cover 90% of daily dev work.**

*English | Beginner Friendly | Free to Share*

[![PowerShell](https://img.shields.io/badge/PowerShell-7%2B-5391FE?logo=powershell&logoColor=white)]()
[![License](https://img.shields.io/badge/License-MIT-green.svg)]()
[![Stars](https://img.shields.io/github/stars/YOUR_USERNAME/powershell-80-20?style=social)]()

</div>

---

## 🤔 What is This?

A **PowerShell cheat sheet** that follows the **80/20 rule** – only the commands you actually use every day.

> Not a "memorize all 500 commands" sheet.
> Just the practical, daily-use commands.

---

## 🎯 Who is This For?

- ✅ Junior / Mid-level Software Engineers
- ✅ DevOps / SRE beginners
- ✅ Anyone learning PowerShell
- ✅ Teams who want a quick reference

---

## 📂 Table of Contents

| # | Section | What it Covers |
|---|---------|---------------|
| 1 | 📁 Navigation | Moving around folders |
| 2 | 🔍 Search & Read | Search logs, read files |
| 3 | 🔗 Pipeline | Filter, Select, Sort |
| 4 | ⚙️ Process | Fix port conflicts |
| 5 | 🌐 Network | Test APIs & connectivity |
| 6 | 📊 Data | Handle JSON/CSV |
| 7 | 📜 Scripting | Write quick scripts |
| 8 | 💡 Pro Tips | Shortcuts & tricks |
| 9 | 🧠 Pattern | Understand the naming convention |
| 10 | 📋 Quick Reference | Everything at a glance |

> **Full details →** [**powershell-80-20.md**](./powershell-80-20.md)

---

## ⚡ Quick Preview

### Port Conflict Fix (Every Dev's Problem)

```powershell
# Port 3000 blocked? Kill it.
$port = Get-NetTCPConnection -LocalPort 3000
stop-process -Id $port.OwningProcess -force
Write-Host "Port free! ✅" -ForegroundColor Green

API Test (1 Line)
# Get user info from GitHub.
$me = irm "https://api.github.com/users/octocat"
$me.name  # → "The Octocat"

Log Search (Like Grep)
# Search "error" in log file.
sls "error" -Path "C:\logs\app.log" -Context 2

Golden Pipeline (Remember This 1 Line)
# Show processes with CPU > 50%, sorted by CPU descending.
Get-Process | ? { $_.CPU -gt 50 } | select Name, CPU | sort CPU -desc

📊 Stats
Metric	Value
Total Commands	40
Covers Daily Work	90%
Time to Learn	2-3 days
Platform	Windows, Mac, Linux (PS 7+)
License	MIT (free to use)

🚀 Getting Started
Step 1: Install PowerShell
# Windows:
winget install Microsoft.PowerShell

# Mac:
brew install --cask powershell

# Linux:
sudo snap install powershell --classic

Step 2: Open the File
1. Download powershell-80-20.md
2. Open in VS Code
3. Press Ctrl+Shift+V (preview)
4. Bookmark it

Step 3: Use It
No need to memorize everything
Look it up when you need it
Get-Help + Google = 5 second solution
🧠 The Pattern – Understand This, Guess 90% Commands
Pattern	Meaning	Example
Get-*	Read / Retrieve	Get-Process
Set-*	Update / Modify	Set-Content
New-*	Create	New-Item
Remove-*	Delete	Remove-Item
Copy-*	Copy	Copy-Item
Move-*	Move / Rename	Move-Item
Test-*	Check / Validate	Test-Path
Start-*	Start	Start-Process
Stop-*	Stop / Kill	Stop-Process
Export-*	Output data	Export-Csv
Import-*	Input data	Import-Csv
Convert-*	Transform format	ConvertTo-Json

One pattern = 90% commands unlocked.

💡 Top 5 Tips
#	Tip	What it Does
1	Tab key	Autocomplete – type half, Tab fills the rest
2	Get-Help cmd	Built-in help for any command
3	Get-Command *x*	Find commands by keyword
4	? and %	Shorthand for Filter and Loop
5	-WhatIf	Preview before executing

🤝 Contributing
✅ Add a new use case
✅ Missing a command? Submit a PR
✅ Improve examples
✅ Found an error? Open an issue
git clone https://github.com/YOUR_USERNAME/powershell-80-20.git
cd powershell-80-20
git checkout -b feature/your-feature
# Edit powershell-80-20.md
git add .
git commit -m "Add: new use case for XYZ"
git push origin feature/your-feature

📄 License
MIT License – Free to use, share, modify.

Full License →

🙏 Acknowledgments
PowerShell Official Docs
PowerShell Cheat Sheet by James Evans
All devs who shared knowledge in communities
⭐ If You Found This Helpful
Give a ⭐ Star. Share with your team.

Platform	What to Do
GitHub	Star ⭐ + Fork 🍴
LinkedIn	Share a post
Twitter/X	Tweet it
Discord/Slack	Drop in team channel
Reddit	r/PowerShell, r/learnprogramming

