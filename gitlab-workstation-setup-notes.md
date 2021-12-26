# GitLab Workstation Setup Notes

# Table of Contents

* [Overview](#overview)
  * [Philosophy and Background](#philosophy-and-background)
* [Setup Notes](#setup-notes)
  * [Basic macOS Setup](#basic-macos-setup)
  * [Required GitLab Tools](#required-gitlab-tools)
  * [GitLab Developer Environment Setup](#gitlab-developer-environment-setup)
    * [GDK Setup](#gdk-setup)
  * [JetBrains IDE Setup](#jetbrains-ide-setup)
    * [Common JetBrains IDE Setup](#common-jetbrains-ide-setup)
    * [RubyMine-Specific IDE Setup](#rubymine-specific-ide-setup)
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
* Log into GitLab-specific Apple ID
* Open App Store and install all updates

## Migration from previous machine

* Manually (via Finder) copy over files from Time Machine backup of old workstation:
  * `~/workspace` (where all my projects are)
  * `~/Desktop` contents as necessary
  * `~/Documents` contents as necessary

## Required GitLab Tools

* 1Password
  * Install and log in to GitLab account. You can use the camera to take a picture of the barcode on your old computer
  * Log into my personal 1Password vault
  * Preferences -> Accounts: remove all keyboard shortcuts (so they don't conflict with JetBrains)
  * Update other settings/vaults as desired
* 

## GitLab Developer Environment Setup

* Install Xcode from app store
* 

### GDK Setup

## JetBrains IDE Setup

### Common JetBrains IDE Setup

### RubyMine-Specific IDE Setup

## Additional Tools

**NOTE: Unless otherwise specified, these are all manually downloaded and installed. I don't use Homebrew casks or other package managers to install and manage macOS `*.app` applications.

* Google Chrome - GitLab-only browser
  * Sign in to GitLab Google account
  * Set as default browser
* Brave - Personal-only browser

