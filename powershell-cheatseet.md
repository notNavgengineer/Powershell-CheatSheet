# ⚡ PowerShell 80/20 – Dev Cheat Sheet

> The **40 most useful PowerShell commands** for everyday development.
>
> Learn the important commands, understand the patterns, and look up the rest when you need them. 🚀

---

# 📁 1. Navigation – Work With Folders and Files

These commands help you **move between folders, view files, and manage files and folders**.

| Command | What It Does                      | When to Use It              | Example              |
| ------- | --------------------------------- | --------------------------- | -------------------- |
| `cd`    | Changes the current folder        | Move to another folder      | `cd C:\Projects\app` |
| `pwd`   | Shows your current location       | Check where you are         | `pwd`                |
| `ls`    | Lists files and folders           | See what is inside a folder | `ls`                 |
| `ls -r` | Lists items inside subfolders too | Search recursively          | `ls -r *.json`       |
| `mkdir` | Creates a folder                  | Create a new folder         | `mkdir src`          |
| `rm`    | Removes a file or item ⚠️         | Delete something            | `rm old.txt`         |
| `cp`    | Copies an item                    | Make a copy                 | `cp a.txt b.txt`     |
| `mv`    | Moves or renames an item          | Change location or name     | `mv old.txt new.txt` |
| `cat`   | Shows file content                | Read a file                 | `cat config.json`    |

### 🧠 Easy Way to Remember

```text
cd    → Change folder
pwd   → Where am I?
ls    → What is here?
cat   → Read the file
cp    → Copy
mv    → Move / Rename
rm    → Remove
```

### Example

```powershell
cd C:\Projects
ls
cd app
cat package.json
```

This means:

```text
Go to Projects
↓
See what is inside
↓
Go to app
↓
Read package.json
```

---

# 🔍 2. Search & Read – Logs and Debugging

These commands are useful when you need to **find errors, inspect logs, or check files**.

| Command             | What It Does                 | When to Use It                | Example                        |
| ------------------- | ---------------------------- | ----------------------------- | ------------------------------ |
| `sls`               | Searches for text            | Find errors or keywords       | `sls "error" app.log`          |
| `Get-Content -Tail` | Shows the last N lines       | Check recent logs             | `Get-Content app.log -Tail 50` |
| `Get-Content -Head` | Shows the first N lines      | Check the beginning of a file | `Get-Content app.log -Head 10` |
| `Test-Path`         | Checks whether a path exists | Verify a file or folder       | `Test-Path C:\data`            |

### Search a Log

```powershell
sls "error" -Path "C:\logs\app.log" -Context 2
```

This searches for `"error"` and also shows nearby lines.

### Read the Latest Logs

```powershell
Get-Content "C:\logs\app.log" -Tail 50
```

Useful when the log file is very large.

### Check Whether a File Exists

```powershell
Test-Path "config.json"
```

`True` means the path exists.

---

# 🔗 3. Pipeline – One Command Feeds Another

The pipeline operator is:

```text
|
```

It sends the output of one command into another command.

Think of it like an assembly line:

```text
Get Data
   ↓
Filter
   ↓
Select
   ↓
Sort
   ↓
Final Result
```

| Command           | What It Does            | Simple Idea              | Example                              |
| ----------------- | ----------------------- | ------------------------ | ------------------------------------ |
| `?`               | Filters items           | Keep only what you need  | `Get-Process \| ? { $_.CPU -gt 50 }` |
| `select`          | Selects properties      | Keep only useful fields  | `Get-Process \| select Name, CPU`    |
| `sort`            | Sorts results           | Put results in order     | `Get-Process \| sort CPU -desc`      |
| `%`               | Runs code for each item | Process items one by one | `ls *.txt \| % { $_.Length }`        |
| `select -First 5` | Takes the first 5 items | Limit the result         | `Get-Process \| select -First 5`     |
| `Out-File`        | Saves output to a file  | Store the result         | `Get-Process \| Out-File list.txt`   |
| `Tee-Object`      | Shows and saves output  | Display + save           | `Get-Process \| Tee-Object list.txt` |

### ⭐ Golden Pattern

```text
Get
 ↓
Filter
 ↓
Select
 ↓
Sort
```

Example:

```powershell
Get-Process |
    ? { $_.CPU -gt 50 } |
    select Name, CPU |
    sort CPU -desc
```

What happens:

```text
Get all processes
↓
Keep processes using more than 50 CPU
↓
Keep only Name and CPU
↓
Sort from high to low
```

### Practical Examples

Find files larger than 1 MB:

```powershell
ls -r |
    ? { $_.Length -gt 1MB } |
    select Name
```

Get the total size of all `.log` files:

```powershell
ls *.log -r | Measure-Object Length -Sum
```

Show output and save it:

```powershell
Get-Process | Tee-Object "processes.txt"
```

---

# ⚙️ 4. Process – Fix Port Conflicts and Hanging Apps

A very common development problem is:

> **"Port 3000 is already in use."**

These commands help you inspect and control running processes.

| Command                | What It Does            | When to Use It            | Example                                |
| ---------------------- | ----------------------- | ------------------------- | -------------------------------------- |
| `ps`                   | Lists running processes | See active processes      | `ps`                                   |
| `Stop-Process`         | Stops a process         | Kill a stuck process      | `Stop-Process -Name "node" -Force`     |
| `Start-Process`        | Starts an application   | Launch a program          | `Start-Process notepad`                |
| `Get-NetTCPConnection` | Shows port connections  | Find what is using a port | `Get-NetTCPConnection -LocalPort 3000` |

### 🔥 Free Port 3000

```powershell
$port = Get-NetTCPConnection -LocalPort 3000

$proc = Get-Process -Id $port.OwningProcess

Write-Host "Port 3000: $($proc.Name) (PID: $($proc.Id))"

Stop-Process -Id $port.OwningProcess -Force

Write-Host "Port free! ✅"
```

The flow is:

```text
Find the port
↓
Find the process
↓
Stop the process
↓
Use the port again
```

---

# 🌐 5. Network – API and Connectivity Testing

These commands help with **REST APIs, ports, and basic network tests**.

| Command              | What It Does            | When to Use It                    | Example                                     |
| -------------------- | ----------------------- | --------------------------------- | ------------------------------------------- |
| `irm`                | Gets data from an API   | Make a REST API request           | `irm https://api.github.com/users/torvalds` |
| `irm -Method POST`   | Sends data to an API    | Make a POST request               | `irm ... -Method POST`                      |
| `Test-NetConnection` | Tests a network port    | Check whether a port is reachable | `Test-NetConnection google.com -Port 443`   |
| `Test-Connection`    | Tests host connectivity | Check whether a host responds     | `Test-Connection google.com -Count 4`       |

### GET Request

```powershell
$me = irm "https://api.github.com/users/octocat"

$me.name
```

The API response can be used like an object.

### POST Request

Create the request body:

```powershell
$body = @{
    title = "Bug fix"
    body  = "Fixed the issue"
} | ConvertTo-Json
```

Send it:

```powershell
irm "https://api.example.com/issues" `
    -Method POST `
    -Body $body
```

### Check a Port

```powershell
Test-NetConnection "myserver.com" -Port 8080
```

Look for:

```text
TcpTestSucceeded : True
```

That means the connection succeeded.

---

# 📊 6. Data – JSON and CSV

JSON is common in APIs. CSV is useful for tables and reports.

| Command            | What It Does                 | When to Use It     | Example                             |
| ------------------ | ---------------------------- | ------------------ | ----------------------------------- |
| `ConvertTo-Json`   | Converts an object to JSON   | Prepare JSON data  | `$data \| ConvertTo-Json`           |
| `ConvertFrom-Json` | Converts JSON into an object | Read JSON data     | `$json \| ConvertFrom-Json`         |
| `Export-Csv`       | Saves data as CSV            | Create reports     | `Get-Process \| Export-Csv out.csv` |
| `Import-Csv`       | Reads a CSV file             | Work with CSV data | `Import-Csv users.csv`              |

### Create JSON

```powershell
$data = @{
    name   = "Rahul"
    skills = @("PowerShell", "Python")
    active = $true
}

$data | ConvertTo-Json -Depth 3
```

### Read API JSON

```powershell
$api = irm "https://api.github.com/users/octocat"

$api.login
$api.public_repos
```

### Create a CSV Report

```powershell
Get-Process |
    select Name, CPU, WorkingSet |
    Export-Csv "report.csv" -NoTypeInformation
```

### Read and Filter CSV

```powershell
Import-Csv "employees.csv" |
    ? { $_.Role -eq "Senior" } |
    select Name, Salary
```

---

# 📜 7. Scripting – Small Automation

PowerShell can also be used to write **small scripts and automate repetitive work**.

## Variables

```powershell
$name = "Rahul"
$files = @("a.txt", "b.txt")

Write-Host "Hello, $name!"
```

A variable stores a value so you can use it later.

---

## If-Else

```powershell
if (Test-Path "config.json") {
    Write-Host "Config found ✅"
}
else {
    Write-Host "Config missing! ❌"
}
```

Think of it as:

```text
Does the file exist?
       │
   ┌───┴───┐
  Yes      No
   ↓        ↓
Found    Missing
```

---

## ForEach Loop

```powershell
Get-ChildItem *.log |
    % {
        Write-Host "$($_.Name) → $($_.Length) bytes"
    }
```

This takes each `.log` file and runs the code once for every file.

---

## Function

A function lets you create reusable logic.

```powershell
function Get-LogSize {
    param(
        [string]$Path = "."
    )

    $total = Get-ChildItem $Path -Filter "*.log" -Recurse |
        Measure-Object Length -Sum

    Write-Host "Total log size: $([math]::Round($total.Sum / 1MB, 2)) MB"
}

Get-LogSize "C:\Projects\app"
```

Now you can call the function whenever you need it:

```powershell
Get-LogSize "C:\Projects\app"
```

---

## Try-Catch – Error Handling

Use `try` and `catch` when an operation may fail.

```powershell
try {
    $data = irm "https://api.example.com/data"

    Write-Host "Data received! ✅"
}
catch {
    Write-Host "API failed: $($_.Exception.Message)"
}
```

Think of it as:

```text
Try
 ↓
Success → Continue
 ↓
Error → Catch it
```

---

# 💡 8. Pro Tips – Useful Shortcuts

| Shortcut / Command | What It Does                             | Why It Helps             |
| ------------------ | ---------------------------------------- | ------------------------ |
| **Tab**            | Autocompletes commands and paths         | Type faster              |
| **↑ Arrow**        | Shows previous commands                  | Reuse old commands       |
| **`?`**            | Short form of `Where-Object`             | Faster filtering         |
| **`%`**            | Short form of `ForEach-Object`           | Faster loops             |
| **`$?`**           | Shows whether the last command succeeded | Check command status     |
| **`$_`**           | Represents the current pipeline item     | Work with one item       |
| **`-WhatIf`**      | Shows what would happen                  | Safely preview an action |
| **`Get-Help`**     | Shows command help                       | Learn a command          |
| **`Get-Command`**  | Finds commands                           | Discover commands        |
| **`2>$null`**      | Hides error output                       | Keep output clean        |

### Get Help

```powershell
Get-Help Get-Process
```

### Find Commands

```powershell
Get-Command *Process*
```

### Preview an Action

```powershell
Remove-Item test.txt -WhatIf
```

---

# 🧠 9. Pattern – Understand PowerShell Command Names

PowerShell commonly uses:

```text
VERB-NOUN
```

The first part describes the **action**.

The second part describes **what the action is working on**.

| Pattern     | Meaning              | Example          |
| ----------- | -------------------- | ---------------- |
| `Get-*`     | Read / retrieve      | `Get-Process`    |
| `Set-*`     | Update a value       | `Set-Content`    |
| `New-*`     | Create something new | `New-Item`       |
| `Remove-*`  | Delete something     | `Remove-Item`    |
| `Copy-*`    | Copy something       | `Copy-Item`      |
| `Move-*`    | Move something       | `Move-Item`      |
| `Test-*`    | Check something      | `Test-Path`      |
| `Start-*`   | Start something      | `Start-Process`  |
| `Stop-*`    | Stop something       | `Stop-Process`   |
| `Export-*`  | Export data          | `Export-Csv`     |
| `Import-*`  | Import data          | `Import-Csv`     |
| `Convert-*` | Convert data         | `ConvertTo-Json` |

### Example

```text
Get-Process
↓
Get = Read
Process = What we read

Stop-Process
↓
Stop = Stop
Process = What we stop

Start-Process
↓
Start = Start
Process = What we start
```

> Once you understand the pattern, unfamiliar PowerShell commands become easier to understand and discover.

---

# 📋 10. One-Line Quick Reference

```text
NAVIGATION
cd, pwd, ls, mkdir, rm, cp, mv, cat

SEARCH
sls, Get-Content -Tail, Get-Content -Head, Test-Path

PIPELINE
?, select, sort, %, Out-File, Tee-Object

PROCESS
ps, Stop-Process, Start-Process, Get-NetTCPConnection

NETWORK
irm, Test-NetConnection, Test-Connection

DATA
ConvertTo-Json, ConvertFrom-Json, Export-Csv, Import-Csv

HELP
Get-Help, Get-Command, Get-Member
```

---

# 📝 Important Notes

## PowerShell 7+

PowerShell 7+ is the modern version to use for current development work.

## Install

```powershell
winget install Microsoft.PowerShell
```

## `curl` vs `curl.exe`

In PowerShell:

```powershell
curl
```

may refer to a PowerShell alias.

To use the native cURL executable:

```powershell
curl.exe
```

## `irm`

`irm` is short for:

```text
Invoke-RestMethod
```

It is useful for working with REST API responses.

## `iwr`

`iwr` is short for:

```text
Invoke-WebRequest
```

It is useful for web requests and response details.

---

# 🎯 The 80/20 Rule

```text
             POWERSHELL
                 │
                 ↓
      Important Commands
                 +
           Useful Patterns
                 │
                 ↓
        Most Daily Work
```

You do not need to memorize every PowerShell command.

Learn the important commands, understand the patterns, practice them, and look up the rest when you need them.

---

<div align="center">

# ⚡ Learn Less. Build More.

**40 Commands → Most Daily Dev Work**

Made with ❤️ for the developer community.

⭐ Found this useful? Star the repository!

</div>
