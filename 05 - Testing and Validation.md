# 05 - Testing and Validation

Each feature was tested in isolation before it went into the main script. This page lists what was checked, how, and what is not covered.

## Contents

- Test approach
- Tests performed
- Accuracy check
- Limitations

## Test approach

1. Run the new command directly in the VS Code PowerShell terminal and inspect the output.
2. Adjust the command or filter until the output is understood.
3. Add only the version that is understood to the script.
4. Save with `Ctrl + S`, run with `F5` and check the output.

For example, the stopped-services filter was run on its own before it was added to `GetSystemInfo.ps1`:

```powershell
Get-Service | Where-Object { $_.Status -eq "Stopped" }
```

Testing a command alone makes it clear what it returns. If a full script misbehaves, the most recently added section is the first suspect.

## Tests performed

| # | Test | Method | Expected | Result |
| --- | --- | --- | --- | --- |
| 1 | Environment check | `Write-Host "Hello from PowerShell!"` in `GetSystemInfo.ps1`, saved and run | Text printed in the terminal | Passed. Confirmed VS Code, the PowerShell extension, the terminal and `.ps1` execution all worked. |
| 2 | Variable interpolation | `$serverName` used inside a double-quoted string | Value substituted into the message | Passed |
| 3 | Dynamic timestamp | Script with `Get-Date` run more than once | Time changes between runs | Passed. The value changed each run. |
| 4 | If / else, warning branch | `simplelogic.ps1` with `$freeSpace = 10` | `WARNING: Disk space is low!` | Passed |
| 5 | If / else, normal branch | `simplelogic.ps1` with `$freeSpace = 50` | `Disk space is fine.` | Passed |
| 6 | Disk threshold on C: | C: free space converted to GB and compared with 10 | No warning when free space is above 10 GB | Passed. Output was `C: Drive Free Space: 48.29 GB` with no warning. |
| 7 | Array loop | `loops.ps1` with three server names | One line per name, in order | Passed |
| 8 | Service status loop | Loop over `Spooler`, `WinRM`, `W32Time`, then re-run with a service that was currently stopped | Output matches the real service state | Passed. The output reflected the actual status. |


<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/d15ce379-c3f2-4076-8b8f-9f46230235a3" />
 The script’s `C: Drive Free Space` line next to the File Explorer *Properties* dialog for the C: drive.

 

## Accuracy check

To confirm the disk space calculation, the figure printed by the script was compared with the free space shown in File Explorer (*This PC*, right-click the C: drive, *Properties*). The numbers matched almost exactly, which confirmed the bytes-to-GB conversion was correct.

## Limitations

These are gaps in testing and in the script itself, recorded so they are not mistaken for finished work.

**Testing scope**

- Developed and run on one Windows 11 laptop with Windows PowerShell 5.1.
- Not run on Windows Server or any other machine. The script is written to be portable (it reads the machine and user name from environment variables), but cross-machine behaviour was not tested.
- No automated tests. Checks were manual.

**Script behaviour**

- No error handling. If a query fails, the script does not recover.
- No null checks. A drive that returns no `Free` value would be calculated as 0 GB and shown as low.
- Output is written with `Write-Host`, so it cannot be piped into other commands or saved as structured data.
- The 10 GB warning threshold is fixed in the script and applies equally to every drive.
- The date format is fixed as `dd/MM/yyyy HH:mm` regardless of regional settings.
- Only low disk space is flagged. Stopped services are listed, not judged.

