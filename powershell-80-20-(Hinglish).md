# ⚡ PowerShell 80/20 – Dev Cheat Sheet

> Sirf woh **40 important commands** jo daily development mein baar-baar kaam aati hain.
>
> Har command ko simple example ke saath samjho, practice karo, aur zarurat padne par isi sheet se wapas dekh lo. 🚀

---

# 📁 1. Navigation – Folders Aur Files Ke Saath Kaam

Sabse pehle yeh seekho ki terminal mein **folder ke andar jaana, current location dekhna, files dekhna aur files/folders manage karna** kaise hota hai.

| Command | Kya Karta Hai                                | Kab Use Karoge                   | Example              |
| ------- | -------------------------------------------- | -------------------------------- | -------------------- |
| `cd`    | Kisi folder mein le jaata hai                | Folder change karna ho           | `cd C:\Projects\app` |
| `pwd`   | Batata hai abhi tum kis folder mein ho       | Current location dekhni ho       | `pwd`                |
| `ls`    | Current folder ki files/folders dikhata hai  | Dekhna ho andar kya hai          | `ls`                 |
| `ls -r` | Subfolders ke andar ki files bhi dikhata hai | Recursively search karna ho      | `ls -r *.json`       |
| `mkdir` | Naya folder banata hai                       | Naya folder banana ho            | `mkdir src`          |
| `rm`    | File ya item delete karta hai ⚠️             | Kuch remove karna ho             | `rm old.txt`         |
| `cp`    | File/item ki copy banata hai                 | Duplicate banana ho              | `cp a.txt b.txt`     |
| `mv`    | File ko move ya rename karta hai             | Location ya naam change karna ho | `mv old.txt new.txt` |
| `cat`   | File ke andar ka content dikhata hai         | File padhni ho                   | `cat config.json`    |

### 🧠 Ek Simple Flow

```text
cd    → Folder mein jao
pwd   → Dekho kahan ho
ls    → Dekho andar kya hai
cat   → File padho
cp    → Copy banao
mv    → Move / Rename karo
rm    → Delete karo
```

### Example

```powershell
cd C:\Projects
ls
cd app
cat package.json
```

Iska simple meaning:

```text
Projects folder mein jao
↓
Uske andar kya hai dekho
↓
app folder mein jao
↓
package.json padho
```

---

# 🔍 2. Search & Read – Logs Aur Debugging

Jab application mein problem aati hai, sabse pehle aksar **logs check** karne padte hain.

| Command             | Kya Karta Hai                                | Kab Use Karoge              | Example                        |
| ------------------- | -------------------------------------------- | --------------------------- | ------------------------------ |
| `sls`               | Text search karta hai                        | Error ya keyword dhundna ho | `sls "error" app.log`          |
| `Get-Content -Tail` | File ki last N lines dikhata hai             | Latest logs dekhne ho       | `Get-Content app.log -Tail 50` |
| `Get-Content -Head` | File ki first N lines dikhata hai            | Starting content dekhna ho  | `Get-Content app.log -Head 10` |
| `Test-Path`         | Check karta hai path exist karta hai ya nahi | File/folder verify karna ho | `Test-Path C:\data`            |

### 💡 Real Example

```powershell
# Log mein "error" dhundo
sls "error" -Path "C:\logs\app.log" -Context 2
```

Yeh `"error"` search karega aur surrounding lines bhi dikhayega.

### Last 50 Lines

```powershell
Get-Content "C:\logs\app.log" -Tail 50
```

Yeh especially useful hai jab log bahut bada ho aur tumhe sirf recent entries dekhni ho.

### File Exist Karti Hai?

```powershell
Test-Path "config.json"
```

Result `True` aaye toh file/path exist karta hai.

---

# 🔗 3. Pipeline – PowerShell Ka Sabse Powerful Concept

Pipeline ka symbol hai:

```text
|
```

Iska kaam simple hai:

> Ek command ka output next command ko de do.

Socho factory assembly line:

```text
Data Lao
   ↓
Filter Karo
   ↓
Required Data Lo
   ↓
Sort Karo
   ↓
Final Result
```

| Command           | Kya Karta Hai                           | Simple Meaning                | Example                              |
| ----------------- | --------------------------------------- | ----------------------------- | ------------------------------------ |
| `?`               | Filter karta hai                        | Sirf required items rakho     | `Get-Process \| ? { $_.CPU -gt 50 }` |
| `select`          | Specific fields leta hai                | Sirf useful information rakho | `Get-Process \| select Name, CPU`    |
| `sort`            | Data ko order karta hai                 | Chhote/bade order mein lagao  | `Get-Process \| sort CPU -desc`      |
| `%`               | Har item par kaam karta hai             | Ek-ek item process karo       | `ls *.txt \| % { $_.Length }`        |
| `select -First 5` | First 5 results leta hai                | Sirf pehle 5 chahiye          | `Get-Process \| select -First 5`     |
| `Out-File`        | Output file mein save karta hai         | Result ko file mein rakho     | `Get-Process \| Out-File list.txt`   |
| `Tee-Object`      | Screen + file dono mein output deta hai | Dekho bhi, save bhi karo      | `Get-Process \| Tee-Object list.txt` |

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

Iska matlab:

```text
Saare processes lao
↓
CPU 50 se zyada wale rakho
↓
Sirf Name aur CPU dikhao
↓
CPU ke hisaab se high → low sort karo
```

### 📌 Real Use Case

1 MB se badi files:

```powershell
ls -r |
    ? { $_.Length -gt 1MB } |
    select Name
```

Log files ka total size:

```powershell
ls *.log -r | Measure-Object Length -Sum
```

Screen par dikhao aur file mein save bhi karo:

```powershell
Get-Process | Tee-Object "processes.txt"
```

---

# ⚙️ 4. Process – Port Conflict Aur Hanging Apps

Development mein common problem:

> **"Port 3000 is already in use."**

Ya application hang ho gayi.

| Command                | Kya Karta Hai                 | Kab Use Karoge                       | Example                                |
| ---------------------- | ----------------------------- | ------------------------------------ | -------------------------------------- |
| `ps`                   | Running processes dikhata hai | Dekhna ho kaunse apps chal rahe hain | `ps`                                   |
| `Stop-Process`         | Process band karta hai        | Stuck process ko stop karna ho       | `Stop-Process -Name "node" -Force`     |
| `Start-Process`        | App/process start karta hai   | Program launch karna ho              | `Start-Process notepad`                |
| `Get-NetTCPConnection` | Port ki information deta hai  | Dekhna ho kaun port use kar raha hai | `Get-NetTCPConnection -LocalPort 3000` |

### 🔥 Port 3000 Free Karna

```powershell
$port = Get-NetTCPConnection -LocalPort 3000

$proc = Get-Process -Id $port.OwningProcess

Write-Host "Port 3000: $($proc.Name) (PID: $($proc.Id))"

Stop-Process -Id $port.OwningProcess -Force

Write-Host "Port free! ✅"
```

Flow:

```text
Port 3000 check karo
↓
Process identify karo
↓
Process stop karo
↓
Port free
```

---

# 🌐 5. Network – API Aur Connectivity Testing

Networking ke basic kaam ke liye kuch commands bahut useful hain.

| Command              | Kya Karta Hai                              | Kab Use Karoge             | Example                                     |
| -------------------- | ------------------------------------------ | -------------------------- | ------------------------------------------- |
| `irm`                | API se data laata hai                      | REST API call karni ho     | `irm https://api.github.com/users/torvalds` |
| `irm -Method POST`   | API ko data bhejta hai                     | POST request bhejni ho     | `irm ... -Method POST`                      |
| `Test-NetConnection` | Port reachable hai ya nahi check karta hai | Server/port test karna ho  | `Test-NetConnection google.com -Port 443`   |
| `Test-Connection`    | Ping jaisa test karta hai                  | Host reachable hai ya nahi | `Test-Connection google.com -Count 4`       |

### GET Request

```powershell
$me = irm "https://api.github.com/users/octocat"

$me.name
```

Yahan API se data aaya aur tum uske fields directly access kar sakte ho.

### POST Request

```powershell
$body = @{
    title = "Bug fix"
    body  = "Fixed the issue"
} | ConvertTo-Json
```

Phir API ko bhejo:

```powershell
irm "https://api.example.com/issues" `
    -Method POST `
    -Body $body
```

### Port Check

```powershell
Test-NetConnection "myserver.com" -Port 8080
```

Agar:

```text
TcpTestSucceeded : True
```

toh connection successful hai.

---

# 📊 6. Data – JSON Aur CSV

Aajkal APIs aur applications mein JSON bahut common hai.

CSV mostly tables/reports ke liye useful hota hai.

| Command            | Kya Karta Hai                        | Kab Use Karoge            | Example                             |
| ------------------ | ------------------------------------ | ------------------------- | ----------------------------------- |
| `ConvertTo-Json`   | Object ko JSON banata hai            | JSON bhejna/save karna ho | `$data \| ConvertTo-Json`           |
| `ConvertFrom-Json` | JSON ko object banata hai            | JSON data read karna ho   | `$json \| ConvertFrom-Json`         |
| `Export-Csv`       | Data ko CSV file mein save karta hai | Report banana ho          | `Get-Process \| Export-Csv out.csv` |
| `Import-Csv`       | CSV file se data laata hai           | CSV read karni ho         | `Import-Csv users.csv`              |

### JSON Banao

```powershell
$data = @{
    name   = "Rahul"
    skills = @("PowerShell", "Python")
    active = $true
}

$data | ConvertTo-Json -Depth 3
```

### API JSON Read Karo

```powershell
$api = irm "https://api.github.com/users/octocat"

$api.login
$api.public_repos
```

### CSV Report Banao

```powershell
Get-Process |
    select Name, CPU, WorkingSet |
    Export-Csv "report.csv" -NoTypeInformation
```

### CSV Filter Karo

```powershell
Import-Csv "employees.csv" |
    ? { $_.Role -eq "Senior" } |
    select Name, Salary
```

---

# 📜 7. Scripting – Chhoti Automation Likho

PowerShell sirf commands chalane ke liye nahi hai.

Tum isse **small scripts aur automation** bhi likh sakte ho.

## Variables

```powershell
$name = "Rahul"
$files = @("a.txt", "b.txt")

Write-Host "Hello, $name!"
```

Variable ka matlab:

> Ek value ko naam dekar store karna.

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

Simple logic:

```text
Agar file hai
   ↓
"Found"

Warna
   ↓
"Missing"
```

---

## ForEach Loop

```powershell
Get-ChildItem *.log |
    % {
        Write-Host "$($_.Name) → $($_.Length) bytes"
    }
```

Meaning:

> Har `.log` file ko ek-ek karke process karo.

---

## Function

Function ka matlab:

> Ek kaam ko reusable bana do.

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

Ab jab bhi log size chahiye:

```powershell
Get-LogSize "C:\Projects\app"
```

---

## Try-Catch

Agar command fail ho jaye toh error handle karne ke liye:

```powershell
try {
    $data = irm "https://api.example.com/data"

    Write-Host "Data mil gaya! ✅"
}
catch {
    Write-Host "API fail hui: $($_.Exception.Message)"
}
```

Socho:

```text
Try karo
   ↓
Success? → Aage badho
   ↓
Error? → Catch handle karega
```

---

# 💡 8. Pro Tips – Yeh Shortcuts Kaam Aayenge

| Shortcut / Command | Kya Karta Hai           | Simple Use                   |
| ------------------ | ----------------------- | ---------------------------- |
| **Tab**            | Auto-complete           | Command/path jaldi likho     |
| **↑ Arrow**        | Previous commands       | Purani command wapas lao     |
| **`?`**            | Filter shortcut         | `Where-Object`               |
| **`%`**            | Loop shortcut           | `ForEach-Object`             |
| **`$?`**           | Last command ka result  | Success hua ya nahi?         |
| **`$_`**           | Current item            | Pipeline mein current object |
| **`-WhatIf`**      | Pehle preview karta hai | Delete/change se pehle check |
| **`Get-Help`**     | Command ki help         | Command samajhne ke liye     |
| **`Get-Command`**  | Commands dhundta hai    | Unknown command search       |
| **`2>$null`**      | Errors hide karta hai   | Unwanted error output chupao |

### Example

```powershell
Get-Help Get-Process
```

Kisi keyword se commands:

```powershell
Get-Command *Process*
```

Pehle check karo command kya karegi:

```powershell
Remove-Item test.txt -WhatIf
```

---

# 🧠 9. Pattern – Command Names Ko Samajhne Ka Shortcut

PowerShell commands aksar:

```text
VERB-NOUN
```

pattern follow karti hain.

Matlab:

```text
Kaam + Kis Cheez Par
```

| Pattern     | Meaning           | Example          |
| ----------- | ----------------- | ---------------- |
| `Get-*`     | Data dekho / lao  | `Get-Process`    |
| `Set-*`     | Value update karo | `Set-Content`    |
| `New-*`     | Naya banao        | `New-Item`       |
| `Remove-*`  | Hatao             | `Remove-Item`    |
| `Copy-*`    | Copy karo         | `Copy-Item`      |
| `Move-*`    | Move karo         | `Move-Item`      |
| `Test-*`    | Check karo        | `Test-Path`      |
| `Start-*`   | Start karo        | `Start-Process`  |
| `Stop-*`    | Roko              | `Stop-Process`   |
| `Export-*`  | Data bahar bhejo  | `Export-Csv`     |
| `Import-*`  | Data andar lao    | `Import-Csv`     |
| `Convert-*` | Format badlo      | `ConvertTo-Json` |

### Example

```text
Get-Process
↓
Process dekho

Stop-Process
↓
Process roko

Start-Process
↓
Process chalao
```

> **Pattern samajh liya toh unknown commands dekhkar bhi unka purpose guess karna easy ho jayega.**

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

### PowerShell 7+

Modern PowerShell ke liye PowerShell 7+ use karna better hai.

### Install

```powershell
winget install Microsoft.PowerShell
```

### `curl` vs `curl.exe`

PowerShell mein:

```powershell
curl
```

alias ho sakta hai.

Actual cURL executable use karna ho:

```powershell
curl.exe
```

### `irm`

`irm` ka full form:

```text
Invoke-RestMethod
```

API ke saath kaam karne ke liye useful hai.

### `iwr`

`iwr` ka full form:

```text
Invoke-WebRequest
```

Web requests aur response details ke liye useful hai.

---

# 🎯 80/20 Rule

```text
        POWERHELL
            │
            ↓
    Core Commands + Patterns
            │
            ↓
     Most Daily Dev Work
```

Tumhe PowerShell ki har command yaad karne ki zaroorat nahi hai.

Bas:

```text
Important Commands
        +
Useful Patterns
        +
Practice
        ↓
Better Productivity
```

---

<div align="center">

# ⚡ Learn Less. Build More.

**40 Commands → Most Daily Dev Work**

Made with ❤️ for the developer community.

⭐ Helpful laga toh Star kar dena!

</div>
