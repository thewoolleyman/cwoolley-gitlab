# GitLab Workstation Setup Notes

# Table of Contents

* [Overview](#overview)
  * [Philosophy and Background](#philosophy-and-background)
* [Setup Notes](#setup-notes)
  * [Basic macOS Setup](#basic-macos-setup)
  * [Required GitLab Tools](#required-gitlab-tools)
  * [GitLab Developer Environment Setup](#gitlab-developer-environment-setup)
    * [GDK Setup](#gdk-setup)
      * [GDK Setup with Rosetta](#gdk-setup-with-rosetta)
      * [GDK Configuration](#gdk-configuration)
  * [Optional/Recommended GitLab Tools/Setup](#optionalrecommended-gitlab-toolssetup)
  * [JetBrains IDE Setup](#jetbrains-ide-setup)
    * [Common JetBrains IDE Setup](#common-jetbrains-ide-setup)
    * [RubyMine-Specific IDE Setup](#rubymine-specific-ide-setup)
      * [RubyMine Plugins](#rubymine-plugins)
      * [RubyMine Project and IDE Config Setup via SCM](#rubymine-project-and-ide-config-setup-via-scm)
      * [RubyMine Manual Setup](#rubymine-manual-setup)
  * [Additional Settings](#additional-settings)
    * [macOS Settings](#macos-settings)
      * [macOS System Preferences Settings](#macos-system-preferences-settings)
      * [macOS Other Settings](#macos-other-settings)
  * [Additional Tools](#additional-tools)

# Overview

This document is notes/instructions for the steps which I ([Chad Woolley](README.md))
took to set up my GitLab developer workstation environment on a 2021 MacBook Pro M1 Max.

This as my only workstation (it is not practical to maintain a second personal workstation,
especially when traveling as a minimalist digital nomad), so I also set up things I require for
personal work and activities, while of course always following all
[GitLab security and acceptable use requirements](https://about.gitlab.com/handbook/people-group/acceptable-use-policy/#acceptable-use-and-security-requirements-of-computing-resources-at-gitlab).

## Philosophy and Background

These are the steps that I used to set up my development workstation.
They are based on [this similar page](https://github.com/thewoolleyman/workstation/blob/master/README.md), which I have used to document the setup
of my personal workstations for the last several years, but this one is specific to my GitLab development environment.


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
**_"just learn to use the defaults, unless they are missing or inconvenient"_**. And, my primary development environment is JetBrains IDEs, not Vim/Emacs/VSCode, which means that
for any given dev ecosystem, almost all basic things "just work out of the box" with
little or no configuration required (yes, I'm a JetBrains stan, feel free to schedule
a meeting with me if you'd like to learn how it can make you more productive ;) )
Anyway, this means that there's relatively few config file entries I need to directly manage,
and therefore it's not worth the overhead to manage it in source control,
but instead just document the entries here. As noted above, I do have a process for
[managing my JetBrains config](https://gitlab.com/jetbrains-ide-config/jetbrains-ide-config-gitlab)
in source control, but this is primarily to let me identify/revert any unexpected breaking changes,
not for setting up a new workstation.

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

* Unbox machine and run through initial macOS setup, i.e. pick username, location, touch ID, etc.
  * NOTE: You should use the same username as the old machine to make things easier.
* Log into GitLab-specific Apple ID
* Open App Store and install all updates
* Perform all [macOS Settings](#macos-settings)
* Install Rosetta. You will be prompted to install it when you start the first app which requires
  it (for me it was JetBrains Toolbox)

## Migration from previous machine

* Manually (via Finder) copy over files from Time Machine backup of old workstation:
  * `~/workspace` (where all my projects are)
  * `~/Desktop` contents as necessary
  * `~/Documents` contents as necessary
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
    * Zip existing dir: `tar -czf ssh_dir.tgz .ssh`
    * Attach file 1password secure note (change dropdown to `file` type)
  * New machine
    * Copy file from 1password secure note
    * Unzip to home dir: `tar -xvf ssh_dir.tgz`

## Required GitLab Tools

* 1Password
  * Install and log in to GitLab account. You can use the camera to take a picture of the barcode on
    your old computer
  * Log into my personal 1Password vault
  * Preferences -> Accounts: remove all keyboard shortcuts (so they don't conflict with JetBrains)
  * Update other settings/vaults as desired
* Slack for Desktop

## GitLab Developer Environment Setup

* Install Xcode from app store
* Install Command Line Tools: `xcode-select --install`

### GDK Setup

* Make sure your SSH key is installed (see instructions above)
* Followed Rosetta instructions based on notes from:
  * https://gitlab.com/gitlab-com/business-technology/engineering/operations/issue-tracker/-/issues/36#note_739788117:
    > To switch to Rosetta mode within terminal (and install Homebrew, asdf, and GDK there), we have two options:
    >
    > 1. Right-click Terminal app icon within Finder, select `Get info`, under `General` section, check `Open using Rosetta` option.
    > 1. Open Terminal normally, and then run `arch -x86_64 /bin/zsh` and it should drop you to prompt where Rosetta is default mode.
    > 1. Verify by running `arch` command and it should show `i386`.
  * https://gitlab.com/gitlab-com/business-technology/engineering/operations/issue-tracker/-/issues/36#note_739795580
* See GDK notes on Silicon setup: https://gitlab.com/gitlab-org/gitlab-development-kit/-/issues/1159

#### GDK Setup with Rosetta

* Set up Terminal app for Rosetta
  * Right-click Terminal app icon within Finder, select Get info, under General section, check Open using Rosetta option
  * Open Terminal normally, and then run `arch -x86_64 /bin/zsh` and it should drop you to prompt where Rosetta is default mode
  * Verify by running arch command and it should show `i386`
* Run GDK one-line installation: https://gitlab.com/gitlab-org/gitlab-development-kit/-/blob/main/doc/index.md#one-line-installation
  * Had some errors, see following for fixes:
    * postgres failed to install, got error `.asdf/plugins/postgres/bin/install: line 45: uconv: command not found`. Had to add:
      * `PATH="/usr/local/opt/icu4c/bin:$PATH"` 
    * https://gitlab.com/gitlab-org/gitlab-development-kit/-/issues/1159#note_796332993 

#### GDK Configuration

* Set up gdk.test as hostname: https://gitlab.com/gitlab-org/gitlab-development-kit/-/blob/main/doc/howto/local_network.md

## Optional/Recommended GitLab Tools/Setup

* TODO: Git command prompt
* TODO: steps from onboarding notes, e.g.:
  * Searching power tips
  * Set up overcommit
* TODO: GitLab dock tool
* TODO: GitLab browser plugins
* TODO: Development Browser Plugins (e.g. vue, vuex, etc)

## JetBrains IDE Setup

### Common JetBrains IDE Setup

* Install JetBrains Toolbox (used to manage all JetBrains IDE installations)
  * Log into Jetbrains account which has licenses for IDEs you use (I pay for and use a personal
    ultimate license which allows me to use any of the IDEs)
* Install all IDEs which you use

### RubyMine-Specific IDE Setup

Note: Ensure you install all plugins FIRST, before configuring setup, so any
plugin-specific configuration entries will be present.

##### RubyMine Plugins

* Utility Plugins
  * .env files support
  * [Lines Sorter](https://plugins.jetbrains.com/plugin/5919-lines-sorter/)
  * GraphQL
  * Prettier
  * TOML
  * Vue.js
  * Makefile Language
  * Ideolog (log files)
  * Files Watcher
* Workflow Plugins
  * New Relic CodeStream - GitHub, GitLab, PRs and Code Review
  * GitLink (open GitLab code/commit links directly from IDE)
* AI Plugins
  * GitHub CoPilot
  * TabNine AI Code Completion
* Keystroke-Learning Plugins
  * Force Shortcuts (if you are hardcore and want to be forced to use keyboard, I leave it disabled)
  * Key Promoter X (if you want to be always reminded to use keyboard)
  * Presentation Assistant (if you are pair programming, and want other people to see what keystrokes you are using)
* Fun Plugins
  * Nyan Progress Bar

#### RubyMine Project and IDE Config Setup via SCM

* I use the following approach to keep my RubyMine GitLab config in source control:
  https://gitlab.com/jetbrains-ide-config/jetbrains-ide-config-gitlab
* However, this may not restore all settings! Even if you use this approach,
  you should still go through the manual setup below to ensure all settings are
  restored

#### RubyMine Manual Setup

All overridden RubyMine settings are documented here:
https://github.com/thewoolleyman/workstation/blob/master/README.md#jetbrains-overridden-settings

I try to keep them updated with the current Jetbrains overridden settings which I use.

You can use these to configure a new RubyMine installation, or to check for
any settings which were missed by the SCM-based setup described above.

## Additional Settings

### macOS Settings

#### macOS System Preferences Settings

* System Preferences - Notifications
  * NOTE: I disable all notifications. This is to prevent distracting popups during screen sharing or
    screen recordings. My current calendar app is the only app I allow to show notifications.
  * System Preferences -> Notifications and Focus
    * Uncheck "Allow Notifications" for all apps except calendar app.
  * ONGOING: Whenever any app pops up a request to show macOS notifications, always choose "Don't Allow"
* System Preferences - Keyboard
  * Max key repeat, shorter delay until repeat.
  * Normal Keyboard: Use function keys as standard function keys
  * Shortcuts -> Services -> Text: Uncheck "Search man Page Index in Terminal" ([steals binding from Jetbrains](https://stackoverflow.com/a/55747595)).
* System Preferences - Trackpad
  * Point and Click -> Force Click and haptic feedback: Turn Off (always causes me to lose highlighting in my way when I'm drag-highlighing with the trackpad)
  * More Gestures: Uncheck "Swipe between pages"
* System Preferences - Battery
  * Battery
    * Turn display off after 10 minutes
    * Turn off "slightly dim the display when on battery power"
  * Power Adapter
    * Turn display off after 1 hour

#### macOS Other Settings

* iCloud
  * Set up iCloud for gitlab ID
    * Set up Find My Mac
    * Uncheck Mail and Calendar
    * Check Keychain and Find My Mac

* Finder
  * Preferences
    * General
      * Show hard disks and connected servers
      * New Finder windows show home directory
    * Sidebar: Show everything
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
  * Show hidden files in Finder: `defaults write com.apple.finder AppleShowAllFiles True; killall Finder` (Cmd-Shift-. (period) to toggle)

* set nopasswd sudoers - `sudo visudo`, change "`%admin ...`" line to read: `%admin   ALL=(ALL) NOPASSWD:ALL`

* Show purchases from personal Apple account in App Store (not positive if this is right!)
  * Set up family sharing for personal apple account and add gitlab account to personal account
  * In App store, log into personal account.
  * In App store, log out of personal account, then log into GitLab account. It should still
    "remember" that you logged into the personal account, and show apps for both IDs under "Purchased By"

* Add `gst` alias to `~/.zshrc`: `alias gst='git status'`

* Add following to `~/.gitconfig`
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
  [core]
  untrackedCache = true
  [push]
  default = current
  [pull]
  rebase = true
  [feature]
  manyFiles = true
  ```

## Additional Tools

**NOTE: Unless otherwise specified, these are all manually downloaded and installed. I don't use Homebrew casks or other package managers to install and manage macOS `*.app` applications.

* Google Chrome - GitLab-only browser
  * Sign in to GitLab Google account via "Turn on Sync"
  * Sync everything but bookmarks (these are done via syncmarx)
  * Set as default browser
  * Important Chrome Extensions (these and others should sync automatically once logged into Google account)
    * Chrome JSON Formatter extension: https://github.com/callumlocke/json-formatter
    * [1password](https://1password.com/downloads/mac/)
    * [syncmarx](https://syncmarx.gregmcleod.com/) bookmark syncer
  * Log into all necessary chrome extensions

* Brave - Personal-only browser
  * Create a "sync chain" on existing old workstation, save it to 1Password, sync to new workstation.
  * Sync everything but bookmarks (these is done via syncmarx)

* iTerm2
  * General -> Selection -> "Characters considered part of word for selection:" -> Remove "`\`"
  * Appearance -> Tabs: Check "Show tab bar even when there is only one tab"
  * Appearance -> Dimming -> Dim background windows
  * Profiles -> Default (starred) Profile -> Keys -> Left Option acts as `Esc+`
  * Profiles -> Default -> General -> Working Directory: Select "Reuse previous session's directory"
  * Profiles -> Default -> Terminal -> Scrollback Buffer -> Scrollback lines: Check "Unlimited Scrollback"

* [Rectangle](https://rectangleapp.com) (window organizer)
  * Give accessibility permissions
  * Remove all default keybindings, add back only (to prevent conflicts with Jetbrains defaults):
    * ctrl-option-cmd ("smash") arrows for top/bottom/right/left
    * smash-m for Maximize
    * smash-n for next display
    * smash-shift-left/up/down/right for top left/top right/bottom left/bottom right
  * Hide menu bar icon, check for updates automatically

* [Clipy](https://clipy-app.com/)
  * Install, launch on system startup.
  * Change Prefs:
    * General -> max clipboard history size 2000
    * General -> Appearance -> Status Bar icon style: none (open via shortcut if you need to access prefs)
    * Menu -> inline: 30, inside folder: 30, characters in menu: 200
    * Change main shortcut Ctrl+Opt+Cmd+V, disable other shortcuts (to not conflict with Jetbrains)

* [Choosy](https://www.choosyosx.com/)
  * Make default browser
  * Rules
    * Disable rule "Some browsers are running"
    * Change default behavior to always open Chrome
    * TODO: Configure apps for personal projects (Rider, Visual Studio) to open in Brave.
  * Advanced: Start at login, don't display icon in menu bar

* [Contexts](https://contexts.co) better window switcher
  * Sidebar: Show sidebar on no display, uncheck auto adjust windows widths
  * Command-Tab: Check "Typing characters starts fast search"

* Adobe Acrobat (Pro if you have a license, Reader if you don't)
  * Pro: Install Adobe Account Access authenticator app

* MeetingBar (from App Store)
  * TODO: Set up, required Calendar setup

* Speedtest App (from App Store)

* Transmit 5 (from App Store)

* iStat Menus (from App Store)
  * Check: Combined
  * Uncheck: Time
  * CPU
    * Active: time graph 
    * App usage Format: 0-100%
  * Network
    * Active: up/down KB/s
  * Sensors
    * Download and install iStat menus helper
  * Combined
    * Items to whow in menubar: CPU & GPU, Network
    * Items to show in dropdown: All

* Kindle (from App Store)

* Krisp
  * TODO: Install

* CotEditor (from app store) - Lightweight editor

* TextMate
  * Preferences -> Projects -> Include Files Matching: `{*,.*}`
  * Preferences -> Terminal _> Install Shell support
  * Configure autosave: add `saveOnBlur = true` to top of `~/Library/Application\ Support/TextMate/Global.tmProperties`

* [Ears](https://retina.studio/ears/) - Quickly switch audio input/output
  * Prefs: Change devices immediately, Display notification when device changes, Start ears at login

* [Timing App](https://timingapp.com) - automatic time tracking by app usage
  * TODO: Configure - sent support email asking about migrating settings

* [Warp](https://warp.dev) terminal (currently in beta, ask for invite if you want to try it)

* Marked (from app store) - Markdown previewer and processor

* Grand Perspective (from app store) - Disk usage visualizer

* VMWare Fusion
  * Publi Tech Preview 21H1 for Apple Silicon
