# Frequently asked questions

## Authentication problems

### Q: I got an error trying to push/pull/clone. What do I do now?

Please follow these steps to diagnose or resolve the problem:

1. Check if you can access the remote repository in a web browser. If you cannot, this is probably a permission problem and you should follow up with the repository administrator for access. Execute `git remote -v` from a terminal to show the remote URL.

1. If you are experiencing a Git authentication problem using an editor, IDE or other tool, try performing the same operation from the terminal. Does this still fail? If the operation succeeds from the terminal please include details of the specific tool and version in any issue reports.

1. Set the environment variable `GCM_TRACE` and run the Git operation again. Find instructions [here](environment.md#GCM_TRACE).

1. If all else fails, create an issue [here](https://github.com/GitCredentialManager/git-credential-manager/issues/create), making sure to include the trace log.

### Q: I got an error saying unsecure HTTP is not supported

To keep your data secure, Git Credential Manager will not send credentials for Azure Repos, Azure DevOps Server (TFS), GitHub, and Bitbucket, over HTTP connections that are not secured using TLS (HTTPS).

Please make sure your remote URLs use "https://" rather than "http://".

### Q: I got an authentication error and I am behind a network proxy

You probably need to configure Git and GCM to use a proxy. Please see detailed information [here](https://aka.ms/gcm/httpproxy).

### Q: I'm getting errors about picking a credential store on Linux

On Linux you must [select and configure a credential store](https://aka.ms/gcm/credstores), as due to the varied nature of distributions and installations, we cannot guarantee a suitable storage solution is available.

## About the project

### Q: How does this project relate to [Git Credential Manager for Windows](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) and [Git Credential Manager for Mac and Linux](https://github.com/Microsoft/Git-Credential-Manager-for-Mac-and-Linux)?

Git Credential Manager for Windows (GCM Windows) is a .NET Framework-based Git credential helper which runs on Windows.
Likewise the Git Credential Manager for Mac and Linux (Java GCM) is a Java-based Git credential helper that runs only on macOS and Linux. Although both of these projects aim to solve the same problem (providing seamless multi-factor HTTPS authentication with Git), they are based on different codebases and languages which is becoming hard to manage to ensure feature parity.

Git Credential Manager (GCM; this project) aims to replace both GCM Windows and Java GCM with a unified codebase which should be easier to maintain and enhance in the future.

### Q: Does this mean GCM for Windows (.NET Framework-based) is deprecated?

Yes. Git Credential Manager for Windows (GCM Windows) is no longer receiving updates and fixes. All development effort has now been directed to GCM. GCM is available as an credential helper option in Git for Windows 2.28, and will be made the default helper in 2.29.

### Q: Does this mean the Java-based GCM for Mac/Linux is deprecated?

Yes. Usage of Git Credential Manager for Mac and Linux (Java GCM) should be replaced with GCM or SSH keys. If you wish to install GCM on macOS or Linux, please follow the [download and installation instructions](../README.md#download-and-install).

### Q: I want to use SSH

GCM is only useful for HTTP(S)-based remotes. Git supports SSH out-of-the box so you shouldn't need to install anything else.

To use SSH please follow the below links:

- [Azure DevOps](https://docs.microsoft.com/en-us/azure/devops/repos/git/use-ssh-keys-to-authenticate?view=azure-devops)
- [GitHub](https://help.github.com/en/articles/connecting-to-github-with-ssh)
- [Bitbucket](https://confluence.atlassian.com/bitbucket/ssh-keys-935365775.html)

### Q: Are HTTP(S) remotes preferred over SSH?

No, neither are "preferred". SSH isn't going away, and is supported "natively" in Git.

### Q: Why did you not just port the existing GCM Windows codebase from .NET Framework to .NET Core?

GCM Windows was not designed with a cross-platform architecture.

### What level of support does GCM have?

Support will be best-effort. We would really appreciate your feedback to make this a great experience across each platform we support.

### Q: Why does GCM not support operating system/distribution 'X', or Git hosting provider 'Y'?

The likely answer is we haven't gotten around to that yet! 🙂

We are working on ensuring support for the Windows, macOS, and Ubuntu operating system, as well as the following Git hosting providers: Azure Repos, Azure DevOps Server (TFS), GitHub, and Bitbucket.

We are happy to accept proposals and/or contributions to enable GCM to run on other platforms and Git host providers. Thank you!

## Technical

### Why is the `credential.useHttpPath` setting required for `dev.azure.com`?

Due to the design of Git and credential helpers such as GCM, we need this setting to make Git use the full remote URL (including the path component) when communicating with GCM. The new `dev.azure.com` format of Azure DevOps URLs means the account name is now part of the path component (for example: `https://dev.azure.com/contoso/...`). The Azure DevOps account name is required in order to resolve the correct authority for authentication (which Azure AD tenant backs this account, or if it is backed by Microsoft personal accounts).

In the older GCM for Windows product, the solution to the same problem was a "hack". GCM for Windows would walk the process tree looking for the `git-remote-https.exe` process, and attempt to read/parse the process environment block looking for the command line arguments (that contained the full remote URL). This is fragile and not a cross-platform solution, hense the need for the `credential.useHttpPath` setting with GCM.

### Why does GCM take so long at startup the first time?

GCM will [autodetect](autodetect.md) what kind of Git host it's talking to. GitHub, Bitbucket, and Azure DevOps each have their own form(s) of authentication, plus there's a "generic" username and password option.

For the hosted versions of these services, GCM can guess from the URL which service to use. But for on-premises versions which would have unique URLs, GCM will probe with a network call. GCM caches the results of the probe, so it should be faster on the second and later invocations.

If you know which provider you're talking to and want to avoid the probe, that's possible. You can explicitly tell GCM which provider to use for a URL "example.com" like this:

Provider|Command
-|-
GitHub|`git config --global credential.https://example.com.provider github`
Bitbucket|`git config --global credential.https://example.com.provider bitbucket`
Azure DevOps|`git config --global credential.https://example.com.provider azure-repos`
Generic|`git config --global credential.https://example.com.provider generic`

### How do I fix "Could not create SSL/TLS secure channel" errors on Windows 7?

This likely indicates that you don't have newer TLS versions available. Please [follow Microsoft's guide](https://support.microsoft.com/topic/update-to-enable-tls-1-1-and-tls-1-2-as-default-secure-protocols-in-winhttp-in-windows-c4bd73d2-31d7-761e-0178-11268bb10392) for enabling TLS 1.1 and 1.2 on your machine, specifically the **SChannel** instructions. You'll need to be on at least Windows 7 SP1, and in the end you should have a `TLS 1.2` key with `DisabledByDefault` set to `0`. You can also read [more from Microsoft](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn786418(v=ws.11)#tls-12) on this change.

### How do I use GCM with Windows Subsystem for Linux (WSL)?

Follow the instructions in [our WSL guide](wsl.md) carefully. Especially note the need to run `git config --global credential.https://dev.azure.com.useHttpPath true` _within_ WSL if you're using Azure DevOps.

### Does GCM work with multiple users? If so, how?

That's a fairly complicated question to answer, but in short, yes. See [our document on multiple users](multiple-users.md) for details.

### How can I disable GUI dialogs and prompts?

There are various environment variables and configuration options available to
customize how GCM will prompt you (or not) for input. Please see the following:

- [`GCM_INTERACTIVE`](environment.md#GCM_INTERACTIVE) / [`credential.interactive`](configuration.md#credentialinteractive)
- [`GCM_GUI_PROMPT`](environment.md#GCM_GUI_PROMPT) / [`credential.guiPrompt`](configuration.md#credentialguiprompt)
- [`GIT_TERMINAL_PROMPT`](https://git-scm.com/docs/git#Documentation/git.txt-codeGITTERMINALPROMPTcode) (note this is a _Git setting_ that will affect Git as well as GCM)

### How can I extend GUI prompts/integrate prompts with my application?

Application developers who use Git - think Visual Studio, GitKraken, etc. - may
want to replace the GCM default UI with prompts styled to look like their
application. This isn't complicated (though it is a bit of work).

You can replace the GUI prompts of the Bitbucket and GitHub host providers
specifically by using the `credential.gitHubHelper`/`credential.bitbucketHelper`
settings or `GCM_GITHUB_HELPER`/`GCM_BITBUCKET_HELPER` environment variables.

Set these variables to the path of an external helper executable that responds
to the requests as the bundled UI helpers do. See the current `--help` documents
for the bundled UI helpers (`GitHub.UI`/`Atlassian.Bitbucket.UI`) for more
information.

You may also set these variables to the empty string `""` to force terminal/
text-based prompts instead.
