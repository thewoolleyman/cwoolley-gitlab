# GitLab Workstation Setup Notes

# Table of Contents

[TOC]

# Overview

This document is notes/instructions for the steps which I ([Chad Woolley](README.md))
took to set up my GitLab developer workstation environment on a 2025 MacBook Pro.

This as my only workstation (it is not practical to maintain a second personal workstation,
especially when traveling as a minimalist digital nomad), so I also set up things I require for
personal work and activities, while of course always following all
[GitLab security and acceptable use requirements](https://about.gitlab.com/handbook/people-group/acceptable-use-policy/#acceptable-use-and-security-requirements-of-computing-resources-at-gitlab).

## Philosophy and Background

These are the steps that I used to set up my development workstation.
They are based on [this similar page](https://github.com/thewoolleyman/workstation/blob/master/README.md),
which I have used to document the setup of my personal workstations for the last several years, but
this one is specific to my GitLab development environment.


These steps are all intended to
be manually performed. They are **_intentionally NOT automated or scripted_**. I also
**_do NOT rely on extensively curated configs or "dotfiles" which are committed to source control_**
(with a couple of exceptions, notably my approach to [managing my JetBrains config](https://gitlab.com/jetbrains-ide-config/jetbrains-ide-config-gitlab)).

I have used automated tools and scripts and dotfiles in the past. I believe those are
useful for shared-workstation pair-programming team environments, where you have to set up new
workstations weekly or monthly. They are also more useful if you are a linux user, heavy
Vim/Emacs/VSCode user, or otherwise use tools which are primarily configured through config
files or dotfiles.

But in my case, for my personal macOS workstations, which I only set up every year or two,
automation is not worth the maintenance overhead, in my experience.

And as far as source controlled config or dotfiles, I try to have the philosophy of
**_"just learn to use the defaults, unless they are missing or inconvenient"_**.
And, my primary development environment is JetBrains IDEs, not Vim/Emacs/VSCode, which means that
for any given dev ecosystem, almost all basic things "just work out of the box" with
little or no configuration required (yes, I'm a JetBrains stan, feel free to schedule
a meeting with me if you'd like to learn how it can make you more productive ;) )

Anyway, this means that there's relatively few config file entries I need to directly manage,
and therefore it's not worth the overhead to manage it in source control,
but instead just document the entries here. As noted above, I do have a process for
[managing my JetBrains config](https://gitlab.com/jetbrains-ide-config/jetbrains-ide-config-gitlab)
in source control. This is primarily to let me identify/revert any unexpected breaking changes,
but it was also handy to save some time setting up Jetbrains a new workstation. However it didn't
capture every config change; some stuff still needed manual re-setup for a new install.

**_NOTE: The exception to the above guidelines is when I copy over application-specific configuration files
from an old machine to a new machine. In many cases, this is simpler and saves time. Specifically,
these are user settings which live under the
[XDG Base Directory Standard directories](https://wiki.archlinux.org/title/XDG_Base_Directory),
and selected MacOS App settings under `$HOME/Library/Application Settings`. See the
["Copy files" section](#copy-files) for more details_**


So, what follows are a list of the steps I've taken and tools I've installed to set up my
GitLab workstation. Not everything is documented in full detail, and I've installed many other
things related to the project du jour which aren't documented here. But in general I try to
remember to update this with most of the important tweaks which I can never remember later,
or may be useful to other people.

I've attempted to categorize them somewhat, to separate things that are likely to be needed
for any gitlab developer from tools/configs that I use personally. 
Within those categories, though, they are mostly chronological/sequential/linear, and later steps
are likely to depend on previous steps.

This helps me, hopefully it helps you!

# Setup Notes

## Basic macOS Setup

* **IMPORTANT** Pick same username for new machine as old machine. This makes copying files easier later.
* Unbox machine and run through initial macOS setup, i.e. pick username, location, touch ID, etc.
  * NOTE: You should use the same username as the old machine to make things easier.
* GitLab will prompt you to register the machine. This will automatically install some apps, currently "Security Software". Chrome, Zoom, 1Password, and Slack. Then it will automatically reboot
* Once it reboots, all of your Chrome settings will automatically sync from your previous machine.
* If it doesn't do it automatically, install the 1Password chrome extension and log in to it.
* Open App Store and install all updates
* Perform all [macOS Settings](#macos-settings)
* Install Rosetta. You will be prompted to install it when you start the first app which requires
  it (for me it was JetBrains Toolbox)

## macOS Settings

### macOS System Settings

Click `Apple` menu on taskbar and `System Settings`

* General
  * Language and Region:
    * Firat Day of the Week: Monday (used by other apps such as Timing and Calendar)
  * Sharing
    * Remote Login -> Turn on. This is required for SSH access - used later to copy files from old workstation via `rsync`.  
* Notifications
  * NOTE: I disable all notifications. This is to prevent distracting popups during screen sharing or
    screen recordings, with the following exceptions:
    * My current native calendar app (currently Apple Calendar)
    * Time-tracking app and plugin (currently Timing)
  * Uncheck "Allow Notifications" for all apps except exceptions listed above.
  * ONGOING: Whenever any app pops up a request to show macOS notifications, always choose "Don't Allow"
* Keyboard
  * Max key repeat, shorter delay until repeat.
  * Keyboard Shortcuts:
    * **!!IMPORTANT! Services -> Text: Uncheck "Search man Page Index in Terminal" ([steals binding from Jetbrains](https://stackoverflow.com/a/55747595)).**
    * Function Keys: Use function keys as standard function keys
  * Change "Press 'globe' key" from default "Show emoji and symbols" to "Do Nothing". Prevents it from popping up when accidentally hitting it instead of
    control on Apple keyboard. Use `Ctrl-Cmd-Space` instead to bring up emoji and symbols "Character Viewer" window.
* Trackpad
  * Point and Click -> Force Click and haptic feedback: Turn Off (always causes me to lose highlighting in my way when I'm drag-highlighing with the trackpad)
  * More Gestures: Uncheck "Swipe between pages"
* Battery
  * Options
    * Turn off "slightly dim the display when on battery power"
  * Power Adapter
    * High Power
* Display
  * Display Settings...
    * Turn off "Automatically Adjust Brightness"
    * Turn off "True Tone"
  * Advanced: Turn off “Allow your pointer and keyboard to move between any nearby Mac or iPad”
* Desktop and Dock - hide, so there's more screen real estate:
  * Size: Medium
  * Magnification: Medium
  * Automatically hide and show the dock

### macOS Other Settings

* iCloud
  * Set up iCloud for gitlab ID
    * Set up Find My Mac
    * Uncheck Mail and Calendar
    * Check Keychain and Find My Mac

* Finder
  * Menu -> View -> Show Path Bar
  * Settings
    * General
      * Show hard disks and connected servers
      * New Finder windows show home directory
    * Sidebar: Show everything except Music and Movies
    * Advanced
      * Show all filename extensions
      * Keep folders on top always (check both boxes)
  * Drag `~/workspace` to sidebar

* Make everything show in Finder:
  * Finder -> home directory
    * Change to List view
    * In Menu-> View -> Show View Options
      * Show Library Folder
      * Use as Defaults (so everything shows in list view)
  * Show hidden files in Finder: `defaults write com.apple.finder AppleShowAllFiles True; killall Finder`
    * **_`Cmd-Shift-.` (period) to toggle hidden files in finder windows_**

* Make file types open in desired app by default. This is useful, for example, when you want `Cmd-Click` on a `*.rb` file link in iTerm
  to automatically open in RubyMine (instead of the default XCode)
  * Open Finder
  * Find some file of the type you want, or just make one, e.g.: `touch /tmp/test.rb`
  * Right click the file and pick `Get info`
  * Expand `Open with`
  * In the dropdown pick `Other`
  * Select the app you want to use. **NOTE: Jetbrains apps like RubyMine installed via Jetbrains Toolbox are in the
    `Applications` directory under your `HOME` directory, _NOT_ the `Applications` directory in the root of the volume.
  * Click `Change all` and confirm
  * Repeat for any other file types you want to automatically open in non-default apps.

* set nopasswd sudoers - `sudo visudo`, change "`%admin ...`" line to read: `%admin   ALL=(ALL) NOPASSWD:ALL`

* Show purchases from personal Apple account in App Store (not positive if this is right, it only has to be done once, not on new machines)
  * Set up family sharing for personal apple account and add gitlab account to personal account
  * In App store, log into personal account.
  * In App store, log out of personal account, then log into GitLab account. It should still
    "remember" that you logged into the personal account, and show apps for both IDs under "Purchased By"

## Set up Developer Tools

These will be required by iTerm2, so go ahead and install them

* Install Command Line Developer Tools: `xcode-select --install`

## Set up iTerm2

iTerm2 is my preferred terminal, so I set it up before moving on to the next steps which require CLI commands.

* iTerm2
  * ITerm2:
    * Make iTerm2 default term
    * Install shell integration
  * Settings:
    * General -> Selection -> "Characters considered part of word for selection:" -> Remove "`\`"
    * Appearance -> Tabs: Check "Show tab bar even when there is only one tab"
    * Appearance -> Dimming -> Dim background windows
    * Profiles -> Default (starred) Profile
      * General -> Initial Directory: Select "Reuse previous session's directory"
      * Text -> Font size 14
      * Terminal -> Scrollback Buffer -> Scrollback lines: Check "Unlimited Scrollback"
      * Keys -> Left Option acts as `Esc+`

## Google Drive setup

* Set up Google Drive syncing
  * See instructions: https://about.gitlab.com/handbook/tools-and-tips/#adding-google-drive-to-your-mac
  * Use "Stream Files" for GitLab account
    * In Finder left pane for drive account, right click "My Drive" -> Offline Access -> 
      Available offline
    * IMPORTANT NOTE: Do NOT have any shortcuts to large "shared folders" in your Google Drive
      "My Drive". This causes Google Drive mirrored offline folders to recurse to subfolders,
      and then it will download files in subfolders and quickly fill up terabytes of local disk space.
      * The workaround for this is to create your own dedicated "shared drive", that is NOT
        under your "My Drive", and move the large shared folders under there for easy access.
  * Set up additional personal account, if desired, using "Mirror Files" if desired. Make it to 
    a custom location like "~/Google Drive Personal Account"

## Migration from previous machine

### Manually copy over /etc/hosts

You can just do this via a Slack or email message to yourself, e.g.

- Old machine: `cat /etc/hosts` > pbcopy
- Paste to yourself
- New machine: `sudo vi /etc/hosts`, paste in contents

### Manually copy over `.ssh` directory

* NOTE: You can store your SSH keys in 1Password, but I don't do this, I keep them on disk.
* Copy over `~/.ssh` directory.
  * Notes on SSH keys
    * I keep my primary ssh key in `~/.ssh/id_cwoolley-gitlab`, and symlink it to `id_rsa`
    * Then on each reboot, I load it automatically with `ssh-add -k` (and type the passphrase)
    * I load other keys (e.g. personal) with `ssh-add ~/.ssh/other_id`. NOTE: The order loaded seems
      to matter sometimes, not sure why!
  * General process: Zip the whole dir, attach to a 1password secure note, then unzip on the new machine (need 1password installed).
    I do keep copies of all individual private keys in 1password, but this is easier to copy everything
    over at once.
  * Old machine
    * Zip existing ssh dir from home dir: `tar -czf yyyy-mm-dd-ssh-dir.tgz .ssh`
    * Attach file 1password secure note (change dropdown to `file` type)
  * New machine
    * Copy file from 1password secure note
    * Unzip to home dir: `tar -xvf yyyy-mm-dd-ssh-dir.tgz`

## Use a tool to bulk copy files from old machine

### Overview

- I prefer to copy my files via `rsync` from old workstation, using `rsync`, because it can restart after
  failures and skip over files which have already been copied.
- However, gitlab security rules block external network connections, which prevents normal incoming ssh connections (and rsync, which uses ssh)  
- So, I use a local separate linux box as a "jump box" to tunnel the SSH traffic between the machines.
  I could use a cloud machine, but then all traffic is going out of my local network which is much slower. You do have an extra linux box
  laying around, right? If not, you should ;). IMPORTANT NOTE: Be sure that you never transfer or store any non-SAFE data
  to a machine which is not owned by GitLab. If you have doubts, use a machine-to-machine solution like AirDrop.
- All commands are on new machine, unless otherwise specified for `Old machine`  

### Copy files

IMPORTANT NOTE: Be sure that you never transfer or store any non-SAFE data to a machine which is not owned by GitLab,
e.g. via the jump box approach shown above. If you have any doubts, use a machine-to-machine solution such as AirDrop.

Keeping that in mind, use can use one of the methods described below (rsync, sftp, PanicTransmit, AirDrop) or another tool of your choice
to bulk copy files.

Files/directories to consider copying:

- `~/workspace` (where I keep all my cloned projects)
- `~/Downloads`
- `~/Documents`
- `~/.zshrc`
- `~/.tool-versions`
- Various settings files and subdirectories (decide on a case-by-case basis whether it makes sense to preserve them or start fresh):
  - MacOS App settings
    `~/Library/Application Settings`
  - [XDG Base Directory Standard directories](https://wiki.archlinux.org/title/XDG_Base_Directory):
    - `~/.config`
    - `~/.local/share`
  - Other dotfiles directly in `~`
    - Specifically, you may want history files. E.g., `ls -al1 ~/ | grep -i history` on old machine, and copy them over.
      Make sure you shut down any apps which use the files so they don't get overwritten on exit or next use.

### Using rsync or sftp to copy files

#### Old machine: put public key in authorized_keys

Ensure you have copied your ssh public key (the same one you copied with your .ssh dir earlier) to `~/.ssh/authorized_keys`:

- `cp id_rsa.pub authorized_keys`
- `chmod 600 authorized_keys`

#### Old machine: reverse tunnel to jump box

- `ssh -R 2222:localhost:22 my-local-linux-machine`
- Leave this terminal running, it's the tunnel connection

#### Old machine: enable ssh

- Go to System Settings -> General -> Sharing -> Remote Login. Ensure it's enabled, and click the `(i)` to verify username is same as new

#### Add the jump box to the SSH config

- Edit `~/.ssh/config`
- Add this (`yourusername` should be the same on both old and new machines):

```
Host old-machine
  HostName localhost
  Port 2222
  User yourusername
  ProxyJump my-local-linux-machine
```

#### Using rsync

* Example rsync command: `rsync -vlaP old-machine:/Users/username/workspace/ ~/workspace/`. Add more `v`s for more verbosity. Add `--stats` for stats at end.
* Using an `rsync` command similar to the above, copy over necessary files from old workstation.

#### Using SFTP

If, instead of rsync, you are using an SFTP-based tool to transfer that does not support `ProxyJump`
(such as [Panic Transmit](https://panic.com/transmit/)), you'll need a different approach:

- On the linux jump box: `ssh -vvv -N -T -g -o ExitOnForwardFailure=yes -L 0.0.0.0:40022:127.0.0.1:2222 localhost`
- Then just use SFTP to connect to `my-local-linux-machine` on port `40022`, which will represent the filesystem of `old-machine`

### Using Airdrop to copy files

* Open the Airdrop app on new machine
* Set the discovery preferences to allow the old machine to see the new machine
* Open Airdrop app on old machine
* Drag files/directories from old machine Finder onto the Airdrop icon for the new machine
* The files will appear under `Downloads` on new machine.

## Required GitLab Tools

* 1Password
  * Install and log in to GitLab account. You can use the camera to take a picture of the barcode on
    your old computer
  * Log into personal 1Password vault in addition to GitLab
  * Settings -> General: remove all keyboard shortcuts (so they don't conflict with JetBrains)
  * Update other settings/vaults as desired
* 1Password for Safari
  * Go to App Store, install for `1Password for Safari`
* Slack for Desktop
  * Sign into GitLab workspace (via Okta)
  * Sign in to any additional workspaces you may have under your personal email (from Brave, sign in to slack.com and choose from list)
* Zoom Settings:
  * Meetings and Webinars:
    * Join experience:
      * Keep my microphone muted: checked
      * Enter full screen automatically: unchecked
    * In-meeting and in-webinar experience -> Use Dual Monitors:  checked
  
## GitLab Developer Environment

### Install Xcode developer tools

* Run `xcode-select --install` (this is a prerequisite for GDK, but listed here seprately).
* Install Xcode IDE from App Store
  * Even though you may not use the Xcode IDE, some things (like GitLab Agent for Kubernetes and thus GitLab Workspaces)
    may need `xcodebuild`, which only comes with full XCode, so go ahead and install it
  * Run `sudo xcodebuild -license accept` to accept the license.

### Install Homebrew

* Go to https://brew.sh/ and copy and run the one-liner install command (this is a prerequisite for GDK, but listed here seprately).
* Follow the instructions printed out to add it to your path. For example (but use the printed version):

```
echo >> /Users/cwoolley/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/cwoolley/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

### GDK Setup

* Make sure your SSH key is installed (see instructions above)
* Make sure you have Xcode developer tools installed (see instructions above)
* Make sure you have Homebrew installed (see instructions above)
* If you will be using nginx (required for GitLab Workspaces), `brew install nginx`

#### If you do not yet have GDK install

* **IF YOU DO NOT YET HAVE A GDK SETUP, do a full installation, via one-line or simple installation methods:
   * Run GDK one-line installation: https://gitlab.com/gitlab-org/gitlab-development-kit/-/blob/main/doc/_index.md#one-line-installation
   * Simple installation: https://gitlab.com/gitlab-org/gitlab-development-kit/-/blob/main/doc/_index.md#simple-installation

#### If you already have a GDK install

* **IF YOU ALREADY HAVE A GDK CLONE COPIED FROM OLD MACHINE, USE THESE INSTRUCTIONS...**

* Change to GDK directory
* `rm <PATH TO GDK>/.cache/.gdk_bootstrapped`
* `rm <PATH TO GDK>/.cache/.gdk_platform_setup`
* `git pull` to get latest commit
* `make bootstrap`

### GDK Configuration

* Set up gdk.test as hostname, loopback, etc: https://gitlab.com/gitlab-org/gitlab-development-kit/-/blob/main/doc/howto/local_network.md
* I also create an additional loopback based on https://gitlab.com/gitlab-org/gitlab-development-kit/-/blob/main/doc/howto/local_network.md#macos,
  for the IP address I have in my local router DHCP for my machine, in `/Library/LaunchDaemons/org.gitlab2.ifconfig.plist`:
  * `sudo cp org.gitlab1.ifconfig.plist org.gitlab2.ifconfig.plist`
  * Use `sudo vi` to edit the file and change the `string` to `...gitlab2`, and the IP to your local network DHCP IP (e.g. `192.168.1.2`)
  * Then change the ownership on it too:

  ```
  sudo chown root:wheel /Library/LaunchDaemons/org.gitlab2.ifconfig.plist
  sudo chmod 644 /Library/LaunchDaemons/org.gitlab2.ifconfig.plist
  ```

### mise configuration

- Symlink the `gitlab` repo's `.tool-versions` file to the home directory, so it applies globally. This helps ensure that even outside
  the gitlab repo, you are always using tool versions which are installed and up-to-date by the latest GDK update.
  - For example: `ln -s ~/workspace/gitlab-development-kit/gitlab/.tool-versions .tool-versions`

## Other CLI setup

* Make `/usr/local/bin` writeable by current user. This is useful for tools which want to drop executables there, such as Jetbrains toolbox.
  - `sudo chmod g+w /usr/local/bin`
  - `sudo dscl . -append /Groups/wheel GroupMembership $USER`
  - Log out and log back in

* Git command prompt
  * I use the `~/.git-prompt.sh` script as described in
    [this blog post](https://about.gitlab.com/blog/2016/12/08/git-tips-and-tricks/#see-the-repository-status-in-your-terminals-prompt),
    with some customizations. 
  * I have the following in my `~/.zshrc`:
    ```sh
    # Git prompt
    GIT_PS1_SHOWCOLORHINTS=true
    GIT_PS1_SHOWDIRTYSTATE=true
    GIT_PS1_SHOWUNTRACKEDFILES=true
    GIT_PS1_SHOWUPSTREAM="auto verbose name git"
     source "$(brew --prefix git)/etc/bash_completion.d/git-prompt.sh"

     # shows branch name
     precmd () { __git_ps1 "%1~" "$ " " (%s)" }
     ```

* Zsh interactive comments
  * Add to shell config to allow comments on pasted command lines:
    ```sh
    # Allow comments on pasted command lines. E.g. `export FOO=bar # note` - don't try to use # note as a filename
    setopt interactive_comments
    ```

* GitLab CLI: https://docs.gitlab.com/ee/editor_extensions/gitlab_cli/#install-the-cli
  * `brew install glab`
* Add `gst` alias in `~/.zshrc`: `alias gst='git status'`
* Install git credential manager: `brew install --cask git-credential-manager`
* Add following to `~/.gitconfig` (this is just my current config, you can choose what parts are relevant)

```
[user]
  name = Chad Woolley
  email = cwoolley@gitlab.com
[alias]
gst = status
ci = commit
co = checkout
sw = switch
up = !git pull --rebase $(git rev-parse --abbrev-ref --symbolic @{u} | sed 's:/: :')
rbi = !git rebase -i $(git merge-base head master)
[credential]
helper = osxkeychain
	helper =
	helper = /usr/local/share/gcm-core/git-credential-manager
[core]
untrackedCache = true
[push]
default = current
[pull]
rebase = true
[feature]
manyFiles = true

[credential "https://dev.azure.com"]
	useHttpPath = true
[credential "http://gdk.test:3000"]
	provider = generic
[maintenance]
	repo = /Users/cwoolley/workspace/gitlab-development-kit
	repo = /Users/cwoolley/workspace/gitlab-development-kit/gitlab
```

## Other GitLab tools setup

* Set up browser to search like a pro with Chrome keyword shortcuts: https://about.gitlab.com/handbook/tools-and-tips/searching/
  * NOTE: If you have your Google Chrome profile sync from a previous machine, this will sync too.
* ~~Set up GitDock: https://about.gitlab.com/blog/2021/10/05/gitpod-desktop-app-personal-activities/~~ (I don't really use this anymore, but it can be useful)

## Additional Apps and Tools

**NOTE: Unless otherwise specified, these are all manually downloaded and installed. I don't use 
Homebrew casks or other package managers to install and manage macOS `*.app` applications, unless otherwise specified.**

### Terminals

* iTerm2 (see iTerm2 instructions above for install and configure)

* [Warp](https://warp.dev) AI terminal

### CLI tools

* (See also this great list of CLI tools from the always-awesome @b0rk (https://twitter.com/b0rk): https://jvns.ca/blog/2022/04/12/a-list-of-new-ish--command-line-tools/)

* 1Password CLI
  * https://developer.1password.com/docs/cli/shell-plugins/gitlab/

* [tldr](https://tldr.sh/) - better man page alternative
  * `brew install tldr`

* [ugrep](https://github.com/Genivia/ugrep#readme) - better grep alternative
  * `brew install ugrep`

* [ripgrep](https://github.com/BurntSushi/ripgrep#installation) - better grep alternative
  * `brew install ripgrep`

* [fzf](https://github.com/junegunn/fzf) - fuzzy finder
  * "It's an interactive Unix filter for command-line that can be used with any list; files, command history, processes, hostnames, bookmarks, git commits, etc."
  * `brew install fzf`
  * Add to `.zshrc`: `source <(fzf --zsh)`

* `k9s` - CLI Kubernetes admin tool - `brew install k9s`

* `kubens` - change kubernetes namespaces - `brew install kubectx`

### Browsers

* Google Chrome - GitLab-only browser
  * Sign in to GitLab Google account via "Turn on Sync"
  * Sync everything but bookmarks (these are done via syncmarx)
  * Set as default browser
  * Important Chrome Extensions (these and others should sync automatically once logged into Google account)
    * Chrome JSON Formatter extension: https://github.com/callumlocke/json-formatter
    * [1password](https://1password.com/downloads/mac/)
    * [syncmarx](https://syncmarx.gregmcleod.com/) bookmark syncer - set up with Dropbox account
  * Log into all necessary chrome extensions

* Brave - Personal-only browser
  * Create a "sync chain" on existing old workstation (Under `Settings -> Sync`), save it to 1Password, sync to new workstation.
  * Sync everything but bookmarks(these is done via syncmarx), reading list, and saved tab groups

* Browser plugins (These will sync automatically through Chrome profile). Some of these are in chrome, some are in Brave, some in both. Depends on usage of the plugin. NOTE: The ones with ~~strikethrough~~ are not currently approved for use in Chrome by GitLab security:
  * [GitLab Conventional Comments button](https://chromewebstore.google.com/detail/gitlab-conventional-comme/hmacdnkefahecfccbjgbjdolhodmnhee)
  * 1Password
  * Apollo Client Devtools
  * Choosy
  * ~~Deluminate~~
  * JSON Formatter
  * Kagi search (optional, I pay for Kagi as an ad-free alternative to Google search engine)
  * Meetings Page Auto Closer for Zoom
  * ~~Snowplow Debugger~~
  * ~~Tab Manager Plus for Chrome~~
  * syncmarx
  * ~~Window Namer and Restorer BETA~~ (No longer in Chrome store, but I'm hoping to write a replacement "someday")
  * Vuejs Devtools
  * [GraphQL Network Inspector](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln/related?hl=en-GB)
  * [GitLab Web Debugger](https://chromewebstore.google.com/detail/gitlab-web-debugger/pjpihpogiidfbgdbmkmmhileaijeakan)

* [Choosy](https://www.choosyosx.com/)
  * Make default browser
  * Rules
    * Disable rule "Some browsers are running"
    * Change default behavior to always open Chrome
    * Add rules as necessary. Or, if migrating from old machine, copy plist file from old machine under `/Users/you/Library/Application Support/Choosy/`
    * (OPTIONAL) Configure apps for personal projects (Rider, Visual Studio) to open in Brave.
  * Advanced: Start at login, don't display icon in menu bar

### Editors

* VS Code

* Cursor editor
  * Cline AI plugin

* [Zed](https://zed.dev/) - Fast Collaborative editor
  - `Install CLI` from menu

* TextMate
  * Settings -> Projects -> Include Files Matching: `{*,.*}`
  * Settings -> Terminal _> Install Shell support
  * Configure autosave: add `saveOnBlur = true` to top of `~/Library/Application\ Support/TextMate/Global.tmProperties`

* CotEditor (from app store) - Lightweight editor
  - `sudo ln -s /Applications/CotEditor.app/Contents/SharedSupport/bin/cot /usr/local/bin/cot`

### Applications

* Docker or other Containerization Tools
  * See the [Containers](#containers) section below for details. 

* Claude Desktop app

* ChatGPT Desktop app

* Bartender 5 - Manage MacOS Menu Items on MacBooks with camera notch
  * Settings -> General
    * Start at login: checked
    * Menu bar item spacing: Tiny
  * Settings -> Menu Bar Items  
    * Drag icons to only show necessary/desired menu items
    * Add menu bar item groups if desired

* [Rectangle](https://rectangleapp.com) (window organizer)
  * Give accessibility permissions
  * Choose "Rectangle" defaults (not Spectacle)
  * If you get "Conflict with macOS tiling", choose "Disable in Rectangle"
  * Remove all default keybindings, add back only (to prevent conflicts with Jetbrains defaults):
    * ctrl-option-cmd ("smash") arrows for top/bottom/right/left halves
    * smash-m for Maximize
    * smash-n for next display
    * smash-shift-left/up/down/right for top left/top right/bottom left/bottom right
  * In config settings (gear icon): Change "Repeated commands" to "Cycle sizes on half actions", and pick  1/2 (default), 2/3, and 1/3.
  * Hide menu bar icon, check for updates automatically (Use Spotlight to show config when needed)

* [Clipy](https://clipy-app.com/)
  * Install, launch on system startup.
  * Change Prefs (click in task bar, it's a clipboard icon):
    * General -> max clipboard history size 2000
    * Menu -> inline: 30, inside folder: 30, characters in menu: 200
    * Change main shortcut Ctrl+Opt+Cmd+V, disable other shortcuts (to not conflict with Jetbrains)

* [Contexts](https://contexts.co) better window switcher
  * Sidebar: Show sidebar on no display, uncheck auto adjust windows widths
  * Command-Tab: Check "Typing characters starts fast search"

* [Clocker](https://apps.apple.com/us/app/clocker/id1056643111?mt=12) from app store
  * Date/Time Zone app for Mac menu bar
  * Add locations, favorite them to show on menu bar
* Adobe Acrobat (Pro if you have a license, Reader if you don't)
  * Pro: Install Adobe Account Access authenticator app

* iStat Menus 7 (from App Store)
  * On install: Check everything but Weather, Power, and Time
  * Check: Combined (last icon with circles)
  * Switch to show stats as desired

* ~~MeetingBar (from App Store)~~ (I don't use this anymore)

* Speedtest App (from App Store)

* Transmit 5 (from App Store)
  * Useful for uploading files to S3/Blobstore

* [Ears](https://retina.studio/ears/) - Quickly switch audio input/output
  * Settings:
    * Shortcut: `Ctrl-Option-Cmd-e`
    *  Change devices immediately, Display notification when device changes, Start ears at login

* [Timing App](https://timingapp.com) - automatic time tracking by app usage
  * (TODO: is this still needed???) Ensure that video calls show notifications, and all Timing notifications are allowed in system 
    settings with highest visibility level.
  * TODO: Configure rest of settings

* Grand Perspective (from app store) - Disk usage visualizer

* ~~Marked (from app store) - Markdown previewer and processor~~ (Don't use this anymore)

* Visual Studio for Mac - For .NET development (F#)

* Mac Calendar App Prefs (required for MeetingBar app)
  * General
    * Start week on: Monday
    * Day starts at: 5AM
    * Day ends at: Midnight
    * Uncheck: show birthdays, show holidays
  * Accounts (ensure you have 1Password safari extension enabled first)
    * Disable iCloud
    * Add Google GitLab account (will need to auth with okta)
    * Add Google personal account

* [Alfred](https://www.alfredapp.com/) - Productivity app
  * Unset `Cmd-Space` from MacOS Default spotlight and reassign to Alfred

* [CleanMyMac](https://cleanmymac.com/) - system health tools 
  * NOTE: If this is a reinstall, go to my.macpaw.com and switch to new device

* [BetterDisplay](https://github.com/waydabber/BetterDisplay) - power user tool for display/resolution settings
  * If you are having issues with an external monitor, or getting MacOS to make a monitor do exactly what you want, this tool can help.
  * Specifically, it can spoof or override EDID attributes, which may be necessary to fix problems
    with MacOS incorrectly mirroring or "forgetting" settings on newer Macs and/or large monitors which are also Smart TVs
  * As noted on Apple’s forums, "Better Display Pro allows you to change the display role from TV to
    Monitor and the annoying mirroring gets fixed." https://discussions.apple.com/thread/255885031?sortBy=rank
  * For my 2024 Macbook and Samsung Odyssey G70NC monitor, I did the following in Better Display preferences:
    * Displays section, expand the section for the monitor
      * System Configuration and Flexible Scaling: Enable
        * Expand "More system configuration options" subsection
          * Edit display role: Enable
          * Edited display role: "Computer monitor"
    * Display EDID Section
      * Use "Retrieve EDID", and copy the base64-encoded string at the bottom
      * Load Custom EDID...
        * Click "Paste Base64" and paste the value from above
        * Click "Apply Custom EDID Now", follow all prompts

* ~~MeetingBar (from App Store) - Meeting reminder app~~ (Don't use this anymore, didn't set up)
  * First, ensure you have calendars set up in Mac calendar app.  If you do, do it, then restart the
    MeetingBar app. Otherwise calendars may not show up.
  * Install options
    * Launch after login screen
    * Send notifications to join meeting: 1 minute before
    * Set up Calendars:
      * Ignore default "Work" and "Home", add work and personal google accounts using default 
        calendar app
      * Add Work calendar from work google account
      * Add Personal calendar from personal google account
  * Preferences -> Appearance
    * Show events for Today and Tomorrow
    * Status Bar icon: Calendar
    * Shorten title to 15 characters
    * Time: Show
    * Show only events starting in 60 minutes
    * Time format: 12 hour
    * Check "details in submenu"

* ~~Cron - Calendar app~~ (I don't use this anymore)
  * cron.com - currently early access, need invite.
  * Connect work and personal accounts
  * Allow notifications

## Other optional apps (Chad's notes on personal tools to install)

* Logseq
* Obsidian
* Discord
* Kindle (from App Store)
* Spotify
* Microsoft Edge
* Steam
  - Go to System Settings → Privacy & Security.
	- Scroll down to Security section — you should see: `<some file> was blocked from use because it is not from an identified developer`
	- Click Open Anyway
* WhatsApp
* Facebook Messenger

## Containers

**IMPORTANT NOTE: Before using Docker Desktop, ensure you are in compliance with the Docker license.**

See the following issue for discussion and links on various/preferred tools and support:

https://gitlab.com/gitlab-com/business-technology/team-member-enablement/issue-tracker/-/issues/1787

See [Docker Desktop section in GitLab handbook](https://about.gitlab.com/handbook/tools-and-tips/mac/#docker-desktop) for details. Some possible tool options are:
* Docker engine with multipass
* Rancher Desktop
* Colima
* nerdctl + lima
* Other???

For my personal usage:
- I installed Rancher Desktop for GitLab-related usage (e.g. Workspaces), using the instructions (using `containerd` at
  https://gitlab.com/gitlab-org/workspaces/gitlab-workspaces-docs/-/blob/main/doc/local-development-environment-setup.md
- I still installed Docker Desktop, using my own license, for use only with personal projects, to provided easy and standardized
  usage of the `docker` CLI and socket, independent of Rancher's container executables installation.
- In Rancher Desktop, I ensure that I do NOT let it manaage my path, and instead manually manage it to ensure it ends up at the end
  of the path rather than the beginning, so other standard `docker` executables take precedence.
- Here's the entry from my `~/.zshrc` (the last line):

```
# Ensure that the official Docker CLI's executables take precedence over Rancher Desktop's,
# by ensuring `~/.docker/bin` gets added to the path BEFORE `~/.rd/bin`.
#
# Ensure that you:
#
# - Configure Docker Desktop to store executables in `~/.docker/bin`
# - Set Rancher Desktop's preferences for `Application -> Environment -> Configure Path` set to "Manual"
export PATH="/Users/cwoolley/.docker/bin:/Users/cwoolley/.rd/bin:$PATH"
```

## Virtual Machine Setup

This section gives some options for running full (non-containerized) Virtual Machines under MacOS.

### Ubuntu Server Under Multipass

NOTE: this is a headless ubuntu server, with no desktop or GUI.

* Download and install multipass: 
  https://multipass.run
* Install an image with multipass: `multipass launch --name linux1`
* Run a shell on the image: `multipass exec linux1 -- bash`

### VirtualBox

* https://www.virtualbox.org/
* 

### Parallels Desktop Pro

* Download and install Parallels Desktop: https://www.parallels.com/products/desktop/download
* In parallels macos menu icon, open "Control Center"
* Click "+" to create a new virtual machine (Ubuntu, Windows 11, or MacOS)
* Notes on using port forwarding to access mac GDK host from ubuntu:
  * See https://linuxize.com/post/how-to-setup-ssh-tunneling/
  * Set up SSH keys appropriately
  * Run port forwarding commands (from ubuntu):
    * `ssh -L 3000:gdk.test:3000 cwoolley@gdk.test` (rails)
    * `ssh -L 2222:gdk.test:2222 cwoolley@gdk.test` (git)
  * Then access at `localhost` from ubuntu

### VMWare Fusion

According to [this Wikipedia article](https://en.wikipedia.org/wiki/VMware_Fusion#History), since Broadcom acquired VMWare, they have made
VMWare Fusion free.

But I'm not going to try it again. Even as a paid tool, VMWare Fusion was still inferior to Parallels the last time I compared them (~4 years ago),
and I can't imagine it's gotten better since the Broadcom acquisition. YMMV...

## JetBrains IDE Setup

**IMPORTANT NOTE: SOME PARTS OF THIS SECTION MAY BE OUT OF DATE: Because I [directly copied over](#copy-files)
all my JetBrains configuration at the filesystem level,
I did not review or update this section when I got my new machine in August 2025. It is still a good reference if you are setting
up JetBrains IDEs your machine from scratch, but know that it may be outdated.**

### UPDATE - See the JetBrains section in the GitLab handbook!

Since the time that I wrote these setup docs, I have created a
section in the GitLab handbook dedicated to Jetbrains IDEs, and their configuration and setup.
There will be additional and updated info there. See:

- [JetBrains IDEs](https://handbook.gitlab.com/handbook/tools-and-tips/editors-and-ides/jetbrains-ides/)
- [Common JetBrains configuration and setup](https://handbook.gitlab.com/handbook/tools-and-tips/editors-and-ides/jetbrains-ides/setup-and-config/)

### Overview

There's a few sub-sections in this "Jetbrains IDE Setup" section. They go in a logical order that will result in the easiest
and smoothest experience. I've tried to indicate which ones are **_REQUIRED_**, **_RECOMMENDED_**, or **_OPTIONAL_**

### Common JetBrains IDE Setup

#### JetBrains Toolbox Installation

I consider the usage of JetBrains Toolbox **_REQUIRED_**. I **_DO NOT_** recommend trying to use Homebrew or manual installation
to manage your JetBrains installations. Toolbox makes everything easier, there's no reason to not
use it.

* Install JetBrains Toolbox (used to manage all JetBrains IDE installations)
  * Log into Jetbrains account which has licenses for IDEs you use (I pay for and use a personal
    ultimate license which allows me to use any of the IDEs, you may use your gitlab JetBrains account)
* Install all IDEs which you use

### RubyMine-Specific IDE Setup

The following sub-sections are mainly designed for RubyMine, but much of it is also
applicable to other JetBrains IDEs, especially when using them within the GitLab
environment.

Note: Ensure you install all plugins FIRST, before configuring setup, so any
plugin-specific configuration entries will be present.

#### Storing RubyMine Project and IDE Config in Git SCM

**NOTE: THIS SECTION DESCRIBES A CUSTOM APPROACH I CAME UP WITH, IT IS _OPTIONAL_ AND _ADVANCED_.
I _DO NOT RECOMMEND_ it for people new to JetBrains.**
But if you are a dedicated JetBrains user, this approach may come in useful across upgrades, and especially when
setting up a new workstation in the future**

* I use the following approach to keep my RubyMine GitLab config in source control:
  https://gitlab.com/jetbrains-ide-config/jetbrains-ide-config-gitlab
* However, this may not restore all settings! Even if you use this approach,
  you should still go through the manual setup below to ensure all settings are
  restored.
* This approachis also kind of flaky. The credentials on the repository sync
  are currently particularly problematic; syncing only seems to work when you
  exit the VM.  

#### RubyMine Plugins

All of these plugins are **__OPTIONAL_** but many are very useful and some could be considered
a **_REQUIREMENT_**. I recommend you review each one and make a decision whether you think it
will be useful for you.

This list changes often for various reasons. Some of these I use often, some I don't. Sometimes,
JetBrains will promote a third-party plugin to a default Jetbrains-provided plugin, or build
it into the IDE. Sometimes plugins are renamed or break in latest versions. But I try to
keep this mostly up to date with my currently curated list. 

TODO: Find a way to easily export this list instead of manually curating it.

* Note on Markdown plugin:
  * The Markdown plugin used to be buggy and slow, but it has gotten better in recent releases.
  * If you use it on main GitLab repos, you will probably want change the setting:
    * Languages and Settings -> Markdown: Automatic assistance in the editor: Turn off
    * Otherwise the plugin will prevent numbering of ordered lists with all `1.`
      and forces them to be sequential.
      See bug: https://youtrack.jetbrains.com/issue/IDEA-292704/Do-not-automatically-number-lists-in-markdown

* Utility Plugins
  * .env files support
  * [Lines Sorter Plus](https://plugins.jetbrains.com/plugin/18564-lines-sorter-plus)
  * Rainbow Brackets - different colored bracket and parenthesis matching (https://plugins.jetbrains.com/plugin/10080-rainbow-brackets)
  * String Manipulation - it, uh, manipulates strings (https://plugins.jetbrains.com/plugin/2162-string-manipulation)
  * Ideolog (log files)
  * Files Watcher
  * [CamelCase](https://plugins.jetbrains.com/plugin/7160-camelcase) - switch between cases with Option+Shift+U
* Workflow Plugins
  * [GitLink](https://plugins.jetbrains.com/plugin/8183-gitlink) - easily open repo web links directly from code
  * [CodeStream (by NewRelic)](https://plugins.jetbrains.com/plugin/12206-new-relic-codestream-github-gitlab-prs-and-code-review) - MRs/PRs and Code
    * In Preferences -> Tools -> Codestream: Turn off "Show line-level blame". It's annoying, I'll open the gutter if I want to see the blame, this isn't VS Code! ;)
    * I ended up disabling this one, I never used it much. I either use the GitLab Web UI or the GitLab Merge Requests plugin. It also seems to crash frequently and clutter the context menus with useless stuff.
  * [GitLab Merge Requests](https://plugins.jetbrains.com/plugin/18689-gitlab-merge-requests) - still in beta and seems buggy, but UI is promising. In-IDE diffs and inline commenting on diffs.
* AI Coding Assistance Plugins
  * GitLab Duo
  * [GitHub CoPilot](https://plugins.jetbrains.com/plugin/17718-github-copilot)
  * [Tabnine](https://plugins.jetbrains.com/plugin/12798-tabnine-ai-code-completion-js-java-python-ts-rust-go-php--more)
* Personal Productivity
  * [CodeTime](https://plugins.jetbrains.com/plugin/15507-codetime) - static analysis and reports of coding patterns
* Keystroke-Learning Plugins
  * Force Shortcuts (if you are hardcore and want to be forced to use keyboard, I leave it disabled)
  * Key Promoter X (if you want to be always reminded to use keyboard)
  * Presentation Assistant (if you are pair programming, and want other people to see what keystrokes you are using)
* Fun Plugins
  * Nyan Progress Bar
* Language/Coding Plugins
  * GraphQL
  * Prettier
  * TOML
  * Vue.js
  * Makefile Language

#### GitLab-specific Rubymine Project Structure Settings

Here is a screenshot of my current Project Structure settings. 

The [exclusions are especially important, because these help reduce indexing time](https://www.jetbrains.com/help/ruby/indexing.html#exclude).

![gitlab-rubymine-project-structure](https://gitlab.com/cwoolley-gitlab/cwoolley-gitlab/uploads/c2a75b6f5b5e88f67ad1acd3dbb6ee7c/gitlab-rubymine-project-structure.png)

#### RubyMine Manual Setup

**If you use RubyMine or other Jetbrains IDEs, I _RECOMMEND_ that you at least review the following
settings, and decide if any are useful for you**

All overridden RubyMine settings are documented here:
https://github.com/thewoolleyman/workstation/blob/master/README.md#jetbrains-overridden-settings

I try to keep them updated with the current Jetbrains overridden settings which I use.

You can use these to configure a new RubyMine installation, or to check for
any settings which were missed by the SCM-based setup described above.
