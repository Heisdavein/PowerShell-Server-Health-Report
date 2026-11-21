# 06 - Lessons Learned

The most useful result of the project was not the finished script. It was the habits picked up on the way from a first command to a report that finds problems and presents them clearly.

## Contents

- Build incrementally
- Test new commands in the terminal first
- Comment as you go
- Avoid hard-coded values
- Understand generated code before running it
- Data compliance
- Working method

## 1. Build incrementally

The project started with `Write-Host "Hello from PowerShell!"`, then added variables, filtering, conditions and loops one at a time. The disk check was not written in one go. It began with reading a single drive, then storing the value, then comparing it with a threshold, then repeating the check for every drive.

Each step stood on something already tested, so it was clear why each line existed, and when something broke the cause was usually in the section added last.

**Takeaway:** build in small, understandable pieces rather than copying a large script that cannot be explained.

## 2. Test new commands in the terminal first

Commands were run in the integrated terminal before they went into `GetSystemInfo.ps1`. Seeing the objects a command returns made the pipeline easier to understand, and the filter could be adjusted before it was committed to the script.

**Takeaway:** test a new command in isolation, understand its output, then add it to the larger script.

## 3. Comment as you go

Each script starts with a header giving its name, purpose, author and last modification date, and smaller comments separate the sections. Comments are most valuable when returning to a script weeks later or handing it to someone else, who should not have to reverse-engineer every line.

**Takeaway:** a few seconds spent documenting a section saves much more time later.

## 4. Avoid hard-coded values

A computer name and user typed into the script would be reported on every machine it ran on. Reading them from `$env:COMPUTERNAME` and `$env:USERNAME` makes the same script report correctly wherever it is run. A script that works on one computer has limited value for IT support.

**Takeaway:** if the system can supply a value, do not type it in.

## 5. Understand generated code before running it

Copilot produced working starting points quickly, but using an assistant is not the same as understanding the code. Before running a generated script that, for example, starts a service, the questions were:

- Which service does it target?
- What happens if the service does not exist, or permission is missing, or the service fails to start?
- Is the error handled and is the output useful?
- Could the command cause an unwanted change to the system?

**Takeaway:** the assistant makes work faster. Understanding the code is what makes the work competent.

## 6. Data compliance is not optional

Cloud-based assistants may receive whatever is pasted into them. For personal learning, fictional examples and test data are fine. At work, credentials, tokens, customer and employee data, confidential company information and sensitive infrastructure details should not go into an external tool unless the organisation has explicitly approved it. Placeholders such as `YOUR_PASSWORD_HERE` and `SERVER01` keep the technical question intact without exposing real values. The full rules are in GitHub and Copilot.

**Takeaway:** if the information could not be posted publicly, it should not be pasted casually into an external service.

## Working method

The routine that came out of the project, reusable for future scripts:

1. **Identify the problem.** What needs collecting or automating?
2. **Find the command.** For example `Get-Service`, `Get-Process`, `Get-PSDrive` or `Get-CimInstance`.
3. **Test it alone.** Run it in the terminal and inspect the result.
4. **Store results in variables** with meaningful names.
5. **Filter** with `Where-Object` and `Select-Object` to keep what matters.
6. **Add decisions** with `if / else` to separate normal from needs-attention.
7. **Add loops** where the same check applies to many items.
8. **Test the script** under different conditions and check the output makes sense.
9. **Handle failures** with proper error handling instead of letting the script stop.
10. **Document and improve.** Add comments, review the code and use assistants carefully where appropriate.

Steps 1 to 8 were applied in this project. 
