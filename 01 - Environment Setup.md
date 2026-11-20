# 01 - Environment Setup

This page records the hardware, software and workspace configuration used for the lab, so the project can be reproduced on similar equipment.

## Contents

- Lab machine
- Software
- Installing Visual Studio Code
- Preparing the workspace
- First script

## Lab machine

The lab ran on an everyday Windows 11 laptop, not a dedicated server. PowerShell has modest hardware requirements, and the machine stayed responsive with VS Code, several PowerShell terminals and browser tabs open at the same time.

| Component | Specification |
| --- | --- |
| Processor | Intel Core i5 (4 cores) |
| Memory (RAM) | 8 GB |
| Storage | 256 GB SSD |
| Operating system | Windows 11 (64-bit) |
| PowerShell version | Windows PowerShell 5.1 (built into Windows 11) |
| Code editor | Visual Studio Code |

> [!NOTE]
The report script is named a “Windows Server Health Report” because it collects the kind of information an administrator checks on a server. It was developed and run on a Windows 11 machine. It was not deployed to a Windows Server installation as part of this lab.
> 

## Software

| Software | Purpose |
| --- | --- |
| Windows PowerShell 5.1 | Scripting environment. Already included with Windows 11, so nothing to install. |
| Visual Studio Code | Code editor for writing and managing the scripts |
| Microsoft PowerShell extension | Syntax highlighting, IntelliSense, debugging and script execution inside VS Code |
| GitHub account | Version control and portfolio hosting. Also required for Copilot |
| GitHub Copilot (free tier) | Coding assistant used for explanations and script review. See GitHub and Copilot |

Everything used is either included with Windows or free to use.

## Installing Visual Studio Code

VS Code was chosen because Microsoft recommends it for PowerShell development, it is widely used in IT, and it provides an integrated terminal, syntax highlighting, IntelliSense and debugging in one window.

1. **Download.** From https://code.visualstudio.com in Microsoft Edge, select *Download for Windows* (stable build). The installer is saved as an `.exe` in the Downloads folder.
2. **Launch the installer.** Accept the User Account Control prompt with *Yes*.
3. **Complete the setup wizard.** Accept the licence agreement and keep the default install location. Enable **Add to PATH** so VS Code can be opened from any terminal with `code`.
4. **Install.** Select *Install*. On this laptop the installation took about 90 seconds.
5. **Launch.** Leave *Launch Visual Studio Code* selected and select *Finish*. The welcome screen confirms the install worked.

>
><img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/51e5a318-7a68-49e2-965a-7ef9413c40c8" />
 Setup wizard on the *Select Additional Tasks* page with *Add to PATH* ticked.


 <img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/1297d783-97f1-441d-8e6e-d500616da683" />
  The VS Code welcome screen after first launch.

## Preparing the workspace

### Integrated terminal

The terminal panel was hidden on first launch. It can be opened from **Terminal > New Terminal** or with `Ctrl + ``. The shell name appears in the dropdown at the top right of the panel. It should read *PowerShell*, not Command Prompt or Git Bash, so that every command in this project behaves as documented.


 <img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/0fadce5b-4b08-4261-b9b6-0081f55417f8" />
  The integrated terminal open with the shell dropdown showing PowerShell.


### Dedicated scripts folder

All scripts live in one folder on the system drive. It was created from the terminal rather than File Explorer, to practise PowerShell at the same time:

```powershell
New-Item -Path "C:\Scripts" -ItemType Directory
```

The new folder was confirmed in File Explorer under *This PC > Local Disk (C:)*.

Keeping scripts together makes them easier to find, group into subfolders later, back up and put under version control. It also lets VS Code focus on a single workspace.

 <img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/533e1659-7eec-4dbe-88e9-5b06a782f8f9" />
   Terminal output of `New-Item` showing the `C:\Scripts` directory entry.
   

### Opening the folder in VS Code

Opening a folder, rather than single files, lets VS Code list every script in the Explorer panel and enables project-wide search and version control integration.

1. *File > Open Folder*
2. Browse to *This PC > Local Disk (C:) > Scripts*
3. Select the folder and choose *Select Folder*

On first open VS Code asks whether to trust the authors of the files in the folder. The folder was created locally by the author, so *Yes, I trust the authors* was selected.

> [!IMPORTANT]
Trusting a folder enables debugging and task execution. In a professional setting, only trust folders that come from known and verified sources.
> 

### PowerShell extension

PowerShell-specific features are not built into VS Code. Microsoft’s official extension was installed from the Extensions view (`Ctrl + Shift + X`): search for *PowerShell*, pick the entry published by Microsoft (verified blue checkmark) and select *Install*. Installation took about 30 seconds.

Visible effects once active:

- Keywords and cmdlets are colour-coded
- IntelliSense suggests commands and parameters while typing
- Syntax errors are highlighted before the script runs
- PowerShell indentation and formatting rules are applied


### VS Code layout

| Area | Purpose |
| --- | --- |
| Menu bar | File, Edit, View, Terminal, Help and other commands |
| Tab bar | Files currently open in the editor |
| Explorer panel | Files and folders in the workspace |
| Editor area | Where scripts are written and edited |
| Integrated terminal | Runs commands and shows script output |
| Extensions panel | Install and manage extensions |

## First script

A new file named `GetSystemInfo.ps1` was created from the Explorer panel. The `.ps1` extension identifies it as a PowerShell script, and VS Code switched to the PowerShell icon and syntax highlighting straight away, confirming the extension was working.

Every script starts with a comment header describing its name, purpose, author and last change:

```powershell
# ============================================================
# Script Name  : GetSystemInfo.ps1
# Description  : Collects and displays Windows server health information
# Author       : <author>
# Last Modified: <date>
# ============================================================
```

The environment was then tested with a single command:

```powershell
Write-Host "Hello from PowerShell!"
```

After saving with `Ctrl + S` and running the script, the terminal printed:

```
Hello from PowerShell!
```

This confirmed that VS Code, the PowerShell extension, the integrated terminal and script execution all worked. The test line was then deleted and the file became the starting point for the health report.


 <img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/4ab0d423-9413-4ad8-b7ff-75c7053cfe93" />
  `GetSystemInfo.ps1` in the editor with the comment header and `Write-Host` line, and the terminal showing `Hello from PowerShell!`.


Next: PowerShell Fundamentals
