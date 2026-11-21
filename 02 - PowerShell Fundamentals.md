# 02 - PowerShell Fundamentals


The health report was built up from four concepts: variables, filtering, decisions and loops. Each one was first tried in isolation, either in the terminal or in a small practice script, and only then added to `GetSystemInfo.ps1`.

## Contents

- Variables
- Filtering with Where-Object
- Decisions with if / else
- Loops with foreach

## Variables

A variable is a named container for a value. Names start with `$`, can contain letters, numbers and underscores, and cannot start with a number.

Tried first in the terminal:

```powershell
# Create a variable
$serverName = "PowerShell"

# Display the value stored in the variable
$serverName

# Use the variable inside a message
Write-Host "The server name is$serverName"
```

```
PowerShell
The server name is PowerShell
```

Inside double quotes PowerShell replaces the variable with its value (variable interpolation). If a value changes, it only needs updating in one place.

### Variables in the script

A variables section was added near the top of `GetSystemInfo.ps1`. The third variable stores the output of a command, not typed text:

```powershell
$computerName = "PowerShell"
$currentUser  = "Administrator"
$today        = Get-Date -Format "dd/MM/yyyy HH\:mm"

Write-Host "Server Name :$computerName"
Write-Host "Reported By :$currentUser"
Write-Host "Report Time :$today"
```

Saving with `Ctrl + S` and running with `F5` printed the name, user and a timestamp. The time changed on every run, which confirmed `Get-Date` was executed each time instead of storing a fixed value.

The two hard-coded values were replaced later. See Script Walkthrough.


 <img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/34551505-da7f-4142-9388-a2064a21af91" />
  The variables block in the editor and the three-line output in the terminal.


## Filtering with Where-Object

Commands such as `Get-Service` and `Get-PSDrive` return dozens or hundreds of objects. A health check needs the exceptions, not everything. `Where-Object` evaluates each object in the pipeline against a condition and passes on only those that match.

```powershell
Get-Service | Where-Object { $_.Status -eq "Stopped" }
```

| Part | Meaning |
| --- | --- |
| `Get-Service` | Returns every service on the computer |
| `\|` | Passes each service object to the next command |
| `$_` | The current object being processed |
| `.Status` | The Status property of that service |
| `-eq "Stopped"` | Keeps only services whose status equals Stopped |

### Comparison operators

| Operator | Meaning | Example |
| --- | --- | --- |
| `-eq` | Equals | `Status -eq "Running"` |
| `-ne` | Not equal to | `Status -ne "Running"` |
| `-gt` | Greater than | `Free -gt 100GB` |
| `-lt` | Less than | `Free -lt 20GB` |
| `-ge` | Greater than or equal to | `CPU -ge 80` |
| `-le` | Less than or equal to | `CPU -le 20` |
| `-like` | Wildcard pattern match | `Provider -like "*FileSystem*"` |
| `-notlike` | Does not match a pattern | Exclude matching text |

### Real file system drives only

`Get-PSDrive` returns more than the physical drives. PowerShell also exposes virtual drives for things such as environment variables, aliases, certificates and functions. These are irrelevant when checking disk space, so the output is filtered on the `Provider` property:

```powershell
$drives = Get-PSDrive |
    Where-Object { $_.Provider -like "*FileSystem*" } |
    Select-Object Name, Used, Free
```

- `like` accepts  as a wildcard for any number of characters, so `"*FileSystem*"` matches `FileSystem` as well as `Microsoft.PowerShell.Core\FileSystem`. `Select-Object` then keeps only the `Name`, `Used` and `Free` properties.

### Stopped services only

```powershell
$stoppedServices = Get-Service |
    Where-Object { $_.Status -eq "Stopped" } |
    Select-Object Name, Status
```

Results are stored in a variable so they can be displayed later and reused.

### Displaying results

```powershell
$drives | Format-Table -AutoSize
$stoppedServices | Format-Table -AutoSize
```

`Format-Table` lays the properties out in columns, and `-AutoSize` adjusts the column widths to fit the terminal.

> **Screenshot Placeholder**
> 
> 

 <img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/192bc9cc-b071-4824-8743-73885f90d1a3" />
 `Get-Service | Where-Object { $_.Status -eq "Stopped" }` run in the terminal with its output.


 <img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/919eaf2a-112b-4996-97dc-32f760473a8e" />
  `$drives | Format-Table -AutoSize` showing only real drives, with Name, Used and Free columns.


## Decisions with if / else

Up to this point the script only reported values. An `if` statement lets it judge them. The syntax was tried in `scripts/practice/simplelogic.ps1`:

```powershell
$freeSpace = 10

if ($freeSpace -lt 20) {
    Write-Host "WARNING: Disk space is low!"
}
else {
    Write-Host "Disk space is fine."
}
```

PowerShell evaluates the condition once and runs exactly one of the two blocks. With `$freeSpace = 10` the output was `WARNING: Disk space is low!`. Changing it to `50` printed `Disk space is fine.`

### Disk space check

Free space on the C: drive is returned in bytes, which is hard to read (`51812614144`). The `1GB` constant converts it, and `[Math]::Round` limits the result to two decimal places:

```powershell
$cDrive = Get-PSDrive -Name C
$freeGB = [Math]::Round($cDrive.Free / 1GB, 2)

if ($freeGB -lt 10) {
    Write-Host "C: Drive Free Space:$freeGB GB [WARNING: LOW DISK SPACE]"
}
else {
    Write-Host "C: Drive Free Space:$freeGB GB"
}
```

The 10 GB threshold was chosen for this lab. A production policy might use 20 GB, a percentage of the drive, or a value based on the server’s role.


 <img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/22a29f21-3a24-4146-a0c1-4f061bd2c87f" />
 `simplelogic.ps1` with `$freeSpace = 10` and the warning output.



<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/5cf21ef3-923d-4643-8d07-de4398defa28" />
 `simplelogic.ps1` with `$freeSpace = 50` and the normal output.


## Loops with foreach

An `if` statement checks one item. A `foreach` loop repeats the same work for every item in a collection, so the script does not need to know in advance how many drives or services exist.

Syntax practice in `scripts/practice/loops.ps1`:

```powershell
$servers = @("web01", "web02", "web03")

foreach ($server in $servers) {
    Write-Host "Checking server:$server"
}
```

```
Checking server: web01
Checking server: web02
Checking server: web03
```

### Looping over services

```powershell
$services = @("Spooler", "WinRM", "W32Time")

foreach ($service in $services) {
    $result = Get-Service -Name $service
    Write-Host "$service is$($result.Status)"
}
```

Each pass retrieves one service, reads its `Status` property and prints it. To check the loop was reading live data, a service that was currently stopped was added to the list, and the output reflected its real status.

### Loop and condition combined

This is the pattern the final script uses for disk space. The loop visits each drive and the `if` decides whether to warn:

```powershell
foreach ($drive in $drives) {

    $freeGB = [Math]::Round($drive.Free / 1GB, 2)

    if ($freeGB -lt 10) {
        Write-Host "Drive$($drive.Name):$freeGB GB [WARNING: LOW DISK SPACE]"
    }
    else {
        Write-Host "Drive$($drive.Name):$freeGB GB"
    }
}
```

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/95ccbcca-805d-4be2-ac5e-3dbca986787e" />
  `loops.ps1` in the editor and the three “Checking server” lines.
.


<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/90021157-f18c-401b-87ad-e42fd60f37f3" />
The services loop output, including the service that was deliberately chosen because it was stopped.

Next: Script Walkthrough
