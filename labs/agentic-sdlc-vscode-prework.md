# Agentic SDLC in VS Code: Pre-work

> **Time:** about 20–30 minutes | **Do this before:** [Lab: Agentic SDLC in VS Code](./agentic-sdlc-vscode.md)

Finish these steps before the session so that the lab time goes to building, not installing.

Work through these steps in order:

1. [Accounts and VS Code](#accounts)
2. [Get the Sample App](#get-the-sample-app)
3. [Install .NET 8](#install-dotnet)
4. [Install Node 24](#install-node)
5. [Run the App](#run-the-app)
6. [Check for a Browser](#browser)
7. [Install the GitHub CLI (Optional)](#install-gh)
8. [Verify](#verify)

<a id="accounts"></a>

## Accounts and VS Code

* A github.com account with **Copilot Business**.
* The latest stable [VS Code](https://code.visualstudio.com/), signed in to GitHub, with GitHub Copilot enabled.
* [Git](https://git-scm.com/downloads), installed and available in the integrated terminal (`git --version`).
* **Copilot Business only:** ask your org admin to enable the **MCP servers in Copilot** policy. It's disabled by default, and Part 6 of the lab needs it.

<a id="get-the-sample-app"></a>

## Get the Sample App

Create a copy of the [dotnet-react-starter-demo repo](https://github.com/copilot-academy/dotnet-react-starter-demo) under your own account. It's a template repository:

1. On the repo page, select **Use this template → Create a new repository**.
2. Choose your account as the owner, keep the name `dotnet-react-starter-demo`, and select **Create repository**.
3. Clone your copy and open it in VS Code. In any terminal (replace `<your-username>`):

   ```bash
   git clone https://github.com/<your-username>/dotnet-react-starter-demo.git
   code dotnet-react-starter-demo
   ```

   If the `code` command isn't found, open VS Code and use **File → Open Folder** to open the `dotnet-react-starter-demo` folder instead. Open the repo folder itself, not its parent folder.

> [!TIP]
> **Use the VS Code integrated terminal**
>
> Every command in this pre-work and the lab runs in the **VS Code integrated terminal**. Open it with **Terminal → New Terminal**, or press `` Ctrl+` ``. Use the **+** button to open more terminals, for example one for the backend and one for the frontend.

<a id="install-dotnet"></a>

## Install .NET 8

The lab's backend, and the tests you generate, need .NET 8. Open the section for your OS:

<details><summary>macOS</summary>

```bash
# Requires Homebrew (https://brew.sh)
brew install dotnet@8

# Homebrew's install folder: /opt/homebrew on Apple silicon, /usr/local on Intel Macs
BREW_PREFIX="$(brew --prefix)"

# Temporarily set environment variables to use dotnet 8 for this session
export DOTNET_ROOT="$BREW_PREFIX/opt/dotnet@8/libexec"
export PATH="$BREW_PREFIX/opt/dotnet@8/bin:$PATH"

# Also make it permanent (optional but recommended)
echo "export DOTNET_ROOT=\"$BREW_PREFIX/opt/dotnet@8/libexec\"" >> ~/.zshrc
echo "export PATH=\"$BREW_PREFIX/opt/dotnet@8/bin:\$PATH\"" >> ~/.zshrc
```

</details>
<details><summary>Windows (PowerShell)</summary>

```powershell
# Install .NET 8 SDK
winget install --id Microsoft.DotNet.SDK.8 --exact

# Verify
dotnet --list-sdks
dotnet --version
```

If `dotnet` isn't recognized after you install it, close and reopen the VS Code terminal (or restart VS Code). If that doesn't fix it, set `DOTNET_ROOT` for the current session:

```powershell
$env:DOTNET_ROOT = "C:\Program Files\dotnet"
$env:Path = "$env:DOTNET_ROOT;$env:Path"
```

Optionally, make these settings persistent, then restart your terminal:

```powershell
[Environment]::SetEnvironmentVariable("DOTNET_ROOT", "C:\Program Files\dotnet", "User")
$userPath = [Environment]::GetEnvironmentVariable("Path", "User")
if ($userPath -notlike "*C:\Program Files\dotnet*") {
  [Environment]::SetEnvironmentVariable("Path", "$userPath;C:\Program Files\dotnet", "User")
}
```

Verify:

```powershell
dotnet --list-sdks
dotnet --version
```

</details>

<a id="install-node"></a>

## Install Node 24

The frontend, the skill scripts, and the Playwright MCP server need Node. Install it with NVM. Open the section for your OS:

<details><summary>macOS</summary>

```bash
# Get the latest version of NVM at https://github.com/nvm-sh/nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
```

Close the terminal and open a new one (the installer updates `~/.zshrc`, which only new terminals load). Then:

```bash
nvm --version
nvm install 24
nvm use 24
node -v
```

</details>
<details><summary>Windows (PowerShell)</summary>

On Windows, install Node 24 with [nvm-windows](https://github.com/coreybutler/nvm-windows). It's a different project from `nvm-sh`.

```powershell
# Install NVM for Windows
winget install --id CoreyButler.NVMforWindows --exact
```

**Restart VS Code** to load the installer's environment changes. Closing just the terminal isn't enough.

Verify `nvm` is on your `PATH`:

```powershell
# PowerShell uses where.exe (not `which`)
where.exe nvm
nvm version
```

If `nvm` still isn't found, run this fallback. It adds the folder that contains `nvm.exe` to your user `PATH`:

```powershell
$nvmHome = "$env:LOCALAPPDATA\nvm"
if (-not (Test-Path "$nvmHome\nvm.exe")) { $nvmHome = "$env:APPDATA\nvm" }

if (-not (Test-Path "$nvmHome\nvm.exe")) {
  throw "nvm.exe not found. Reinstall nvm-windows, then restart VS Code."
}

$nvmSymlink = "$env:ProgramFiles\nodejs"

[Environment]::SetEnvironmentVariable("NVM_HOME", $nvmHome, "User")
[Environment]::SetEnvironmentVariable("NVM_SYMLINK", $nvmSymlink, "User")

$userPath = [Environment]::GetEnvironmentVariable("Path", "User")
if ($userPath -notlike "*$nvmHome*") {
  $userPath = "$userPath;$nvmHome"
}
if ($userPath -notlike "*$nvmSymlink*") {
  $userPath = "$userPath;$nvmSymlink"
}
[Environment]::SetEnvironmentVariable("Path", $userPath, "User")

# Make it work in the current window too
if ($env:NVM_HOME -ne $nvmHome)    { $env:NVM_HOME    = $nvmHome }
if ($env:NVM_SYMLINK -ne $nvmSymlink) { $env:NVM_SYMLINK = $nvmSymlink }
if ($env:Path -notlike "*$nvmHome*")    { $env:Path = "$nvmHome;$env:Path" }
if ($env:Path -notlike "*$nvmSymlink*") { $env:Path = "$nvmSymlink;$env:Path" }

where.exe nvm
nvm version

Write-Host "If this succeeds, restart VS Code once so future terminals inherit the same PATH."
```

Then install and use Node 24:

```powershell
nvm install 24.0.0
nvm use 24.0.0

# Verify
node -v
npm -v
```

> [!NOTE]
> If you installed Node directly with the MSI installer, uninstall it before using nvm-windows to avoid conflicts.

</details>

<a id="run-the-app"></a>

## Run the App

1. In one integrated terminal, build and run the backend:

<details><summary>macOS</summary>

```bash
cd backend
dotnet restore
dotnet build
ASPNETCORE_ENVIRONMENT=Development dotnet run --project src/Api
```

</details>
<details><summary>Windows (PowerShell)</summary>

```powershell
cd backend
dotnet restore
dotnet build
$env:ASPNETCORE_ENVIRONMENT = "Development"
dotnet run --project src/Api
```

</details>

2. In a second integrated terminal (select **+**), run the frontend:

```bash
cd frontend
npm install
npm run dev
```

3. Open http://localhost:5000 (API) and http://localhost:5173 (UI) to confirm both are running. Leave both terminals running for the lab.

<a id="browser"></a>

## Check for a Browser

Part 6 uses the Playwright MCP server, which drives the **Google Chrome** installed on your machine. If you don't have Chrome:

* Install [Google Chrome](https://www.google.com/chrome/), **or**
* On Windows, plan to use Microsoft Edge instead. The lab's [Troubleshooting](./agentic-sdlc-vscode.md#troubleshooting-playwright-browser) section shows the one-line change.

<a id="install-gh"></a>

## Install the GitHub CLI (Optional)

Part 3 uses `gh skill` from the GitHub CLI, version **2.90.0 or later**, to install a public community skill. This is the general-purpose GitHub CLI, not Copilot CLI. If you can't install it, Part 3 has a manual `git clone` alternative.

<details><summary>macOS</summary>

```bash
brew install gh
gh --version
gh auth login
```

</details>
<details><summary>Windows (PowerShell)</summary>

```powershell
winget install --id GitHub.cli
# Open a new terminal so gh is on PATH, then:
gh --version
gh auth login
```

</details>

See [Installing gh](https://github.com/cli/cli#installation) for other options.

<a id="verify"></a>

## Verify

Run these in the VS Code integrated terminal. You're ready when every check passes:

- [ ] `dotnet --version` prints `8.x`.
- [ ] `git --version` succeeds.
- [ ] `node --version` prints `v24.x`.
- [ ] `gh auth status` shows you're signed in (optional).
- [ ] Your copy of the sample repo is open in VS Code as the workspace folder.
- [ ] http://localhost:5000 (API) and http://localhost:5173 (UI) both respond.
- [ ] Google Chrome is installed (or, on Windows, you'll use Edge).
- [ ] The Chat view opens and you're signed in to Copilot.

If a port is busy or something else fails, see [Troubleshooting](./agentic-sdlc-vscode.md#troubleshooting) in the lab.

**Done?** Continue to the [lab](./agentic-sdlc-vscode.md) at the start of the session.
