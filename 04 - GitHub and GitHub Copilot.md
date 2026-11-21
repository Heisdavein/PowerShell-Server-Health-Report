# 04 - GitHub and GitHub Copilot

Once the scripts worked locally, the next step was to manage them the way an administrator would: stored somewhere other than one laptop, with a change history. This page covers the GitHub account setup, how GitHub Copilot was used during the project, and the data-handling rules followed while using it.

## Contents

- Why GitHub
- Account setup
- GitHub Copilot
- Data handling rules
- Installing Copilot in VS Code
- How Copilot was used
- Working rule

## Why GitHub

GitHub is built on Git, a version control system. Unlike a file sync service, it keeps a history of changes. If an edit introduces a bug, the earlier working version can be compared against and restored.

Reasons for using it on a solo project:

- Scripts have a home outside the local machine and a better backup
- Every change is recorded and earlier versions can be reviewed
- Projects can be shown as part of a technical portfolio
- It practises workflows used in professional IT and development teams

## Account setup

1. Open https://github.com and select *Sign up*.
2. Enter an email address, a strong password and a username. The username was chosen to be suitable for a CV or LinkedIn profile.
3. Verify the email address using the link GitHub sends.
4. Review the profile and security settings, add a profile picture and **enable two-factor authentication (2FA)**.

2FA matters because the account holds the code and forms part of a public portfolio. A strong password alone is a single point of failure.


## GitHub Copilot

GitHub Copilot is a coding assistant that integrates with VS Code. It can suggest code as you type, generate code from a plain-English description, explain unfamiliar code, review scripts and suggest alternatives. It was treated as a colleague to ask questions, not as something that takes over responsibility for the code.

The free tier was used. At the time of the lab it allowed up to 50 chat requests and 2,000 code completions per month, which was enough for the exercises here. Plans and limits change, so check GitHub’s current Copilot plan information before relying on these figures.

Copilot was activated from https://github.com/features/copilot by selecting *Get started for free*.

## Data handling rules

Information typed into an external service should not be assumed to stay on the local machine. How prompts and code are handled depends on the service, account, settings and policy. The rule adopted:

> If I would not be comfortable posting the information publicly, I should not casually paste it into an external assistant.
> 

| Category | Do not share | Use instead |
| --- | --- | --- |
| Credentials | Passwords, API keys, access tokens, private keys, database credentials, authentication secrets | `YOUR_PASSWORD_HERE` |
| Infrastructure | Real production server names, production IP addresses, internal network diagrams, firewall configuration, sensitive config files | `SERVER01`, `DC01` |
| Personal data | Employee or customer names, email addresses, phone numbers, financial information, employee records | `USER_NAME`, `EMPLOYEE_ID`, `USER_EMAIL` |
| Company information | Confidential project or client information, internal documentation, proprietary source code, internal security procedures | Do not paste unless the organisation has explicitly approved it |

At work, the organisation’s IT security and data-handling policy comes first. A tool can be acceptable for some tasks and prohibited for others.

## Installing Copilot in VS Code

1. Open the **Extensions** view in the left sidebar.
2. Search for `GitHub Copilot`, select the extension published by GitHub and choose *Install*.
3. Select the Copilot icon, choose *Sign In*, then *Continue with GitHub*.
4. In the browser, review the request and select *Authorize Visual Studio Code*.
5. When prompted, choose *Open* to return to VS Code. VS Code then shows the GitHub account as connected.

The environment at this point consisted of PowerShell, VS Code, the PowerShell extension, GitHub and GitHub Copilot.


<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/15d46586-5720-42ba-95cb-a45d88c53e1e" />
VS Code showing GitHub Copilot installed and the GitHub account connected.


## How Copilot was used

### Generating a script

Prompt quality changes how useful the result is. A vague request leaves too much open:

```
Write me a PowerShell script to check a service.
```

It does not say which service, whether to only report or also act, how to handle a missing service, or where output should go. A more specific prompt does:

```
Write me a PowerShell script that checks if the Windows Update service
is running and displays the result on screen. If the service is stopped,
automatically try to start it and confirm whether that was successful.
```

The generated script was **read before it was run**. Because variables, filtering, conditions and loops had already been learned and tested, the output could be questioned line by line:

- What does this line do and why is this command used?
- What happens if the service does not exist?
- What happens if PowerShell cannot start the service?
- Is the error handling sufficient?
- Does the script behave the way it was asked to?

### Reviewing an existing script

The most useful application was reviewing code already written. The prompt was:

```
Can you review this PowerShell script and suggest improvements?
```

The suggestions that came back:

| Suggestion | Reasoning |
| --- | --- |
| Avoid relying on `Write-Host` for data | Structured objects can be passed down the pipeline and reused by other commands. `Write-Host` is fine for display, but limits automation. |
| Use a consistent date format | Date output can vary with regional settings. A fixed format is easier to read, document and process. |
| Check for null or missing values | Commands can return no result, a missing property or an unexpected object. Calculations should not assume the data is there. |
| Keep warnings visually meaningful | If every line is highlighted, real warnings are harder to see. Reserve strong emphasis for what needs attention. |
| Add error handling | Use `try/catch` where commands can fail, so the script handles problems instead of terminating unexpectedly. |

None of these were applied to the final script. They are carried forward as future improvements.


 <img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/2c3b6d14-9127-40a4-ade7-79e3742c29a8" />
  The Copilot chat panel showing the review prompt and the list of suggestions.

  

## Working rule

AI-generated code is a proposal, not an answer. The workflow followed was:

```
Ask -> Read -> Understand -> Test -> Verify -> Improve
```

and not:

```
Ask -> Copy -> Run -> Hope
```

The central principle: Copilot can help write code faster, but it does not replace the responsibility to understand the code. A script that cannot be explained, troubleshot or modified by its author has not been learned.

Next: Testing and Validation
