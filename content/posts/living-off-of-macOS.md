---
title: "Living off the land in MacOS"
date: 2019-09-21T22:24:42-07:00
draft: true
tags: ['macOS']
noSummary: false
---

I've been spending quite some time recently going over macOS security features and thisshould be the first of _many_ posts discussing that research. In this post I'm going to talk about the built-in tools that macOS users/hackers ought to know.

#### dscl
`dscl` is the command line interface for the macOS directory services authentication framework. It can list, write and read information regarding the users on the local machine, or about the directory if deployed in a LDAP envrioment.

Example uses:

* `dscl . -
* `dscl . -read /Users/$(whoami)`
* `dscl . -write /`TODO
* `dscl . -read /Users/`whoami` LinkedIdentity`

#### log
Starting in macOS 10.12 (High Sierra), the logging system shifted to using `os_log` API for the system logging which reduced the number of logs written to disk as they would be stored in memory. The `log` command or Console.app can be used to read these logs. The default behavior of the log command can be altered by using a `~/.logrc` file in the users home.

Example uses:

* `log [show]|[stream] -last 1h --predicate 'subsystem == "com.apple.securityd"'`
* `log `


#### csrutil
`csrutil` is the interface to macOS's System Intergrity Protection (SIP). Changing the settings from this utility can only be done when the machine is booted into recovery mode.

Example uses:

* `csrutil status` - The default value is enabled.
* `csrutil [enable]|[disable]` - Can only be used from recovery mode.
* `csrutil netboot [add]|[delete] IPv4` - Adds a macOS NetBoot server. NetBoot servers add scripts that can be run versus full installs.

#### spctl


#### launchctl


#### plutil
Apple stores many configuration files as property lists which can be be stored as XML, JSON, or a binary format. `plutil` can lint, display and modify these `.plist` files.

Examples:

* `plutil <file>.plist` - Lint the sytax of the file.
* `plutil -p <file>.plust` - Print the file to stdout.
* `plutil -convert [xml]|[json] <file>.plist` Converts files to a new format.

#### scutil
Provides a CLI to the data stored by configd(8).

Examples: 

* `scutil --dns` - Grab DNS configuraion for the system
* 


#### security

#### bless


### Credit

This research is a culimnation of testing things, and reading material by the following folks:

* [Patrick Wardle](https://twitter.com/patrickwardle)
* [J Levin](https://twitter.com/Morpheus______)
* [Howard Oakley](https://twitter.com/howardnoakley)
