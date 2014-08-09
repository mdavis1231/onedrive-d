# onedrive-d

A Microsoft OneDrive client on Linux desktop environment, written in Py3k.

## Introduction

The branch `1.0-dev` will be rewritten, thereby finishing the features
that are unable or itchy to implement in old releases.

### TODO Lists

 - [ ] Live Connect REST API written in Py3k
 	 - [X] Get access token uri
 	 - [X] Get access token
 	 - [X] Refresh access token
 	 - [X] Sign out
 	 - [ ] Download a file
 	 - [ ] Upload a file
 	 - [ ] Update a file
 	 - [ ] Getting user data
 	 - [ ] Obtaining user consent
 	 - [ ] Read file property
 	 - [X] Read folder property
 	 - [ ] Update file property
 	 - [ ] Update folder property
 	 - [ ] Get links to files and folders
 	 - [X] Get the storage quota information
 	 - [ ] Get a list of shared files / folders
 	 - [ ] Move or copy a file or folder
 	 - [ ] Delete a file
 	 - [ ] Create a folder
 	 - [ ] Delete a folder
 	 - [X] Get a list of most recently used documents
 	 - [X] Traverse the OneDrive directory
 	 - [ ] Display a preview of an OneDrive item
 	 - [ ] Create an album
 	 - [ ] Update an album
 	 - [ ] Read an album
 	 - [ ] Delete an album
 	 - [ ] Read a tag
 	 - [ ] Create a tag
 	 - [ ] Delete a tag
 - [X] Support both GUI and command-line interfaces
 	 - [X] Command-line preference dialog
 	 - [ ] Gtk preference Dialog
 	 - [ ] Command-line observer
 	 - [ ] Gtk observer
 - [X] Easily customizable ignore list
 - [ ] Installation scripts

## Installation

Run command `./setup.sh` (planned)

## Uninstallation

Run command `./setup.sh uninstall` (planned)

## Approaches

The program consists of two parts, *main* and *prefs*. Both parts can run with and without GUI The program can run with and without GUI.

The GUI library planned to use is `PyGI`.

### Architecture

There are two entry points in the program: `onedrive-d` and `onedrive-pref`:

 * `onedrive-d` deals with synchronization.
 * `onedrive-pref` updates the preferences.

Both commands support two arguments: `--no-gui` (use command-line interface) and `--help` (list all supported arguments).

Besides, `onedrive-pref --log-out` will log out the current user.

#### onedrive-d

For `onedrive-d`, there are three major threads:

 * The *MainThread* initializes the process and communicates with OS.
 * The *daemon* thread syncs the local repository with the OneDrive server and issues events to its observers;
 * The *observer* thread observes and handles events from the daemon.
 	 * The observer may be hooked with either `default` or `gtk` event handlers, depending on the existence of `--no-gui`.

#### onedrive-pref

## FAQ

### Why are my files / dirs renamed to add *_conflict?

For case conflicts, since NTFS is case-INsensitive, the local 
repository cannot have two files whose names only differ in cases, say, `hello.c`
and `Hello.c`. In this case one of them will be renamed `hello (case_conflict_1).c`
and then get synced.

For type conflicts, if the remote entry and local entry have the same
name but different types, say, the local path `~/OneDrive/doc` is a file
while in OneDrive server `/doc` is a folder, the local one will get renamed.

### What is the reference environment?

The program is tested on latest Xubuntu 64-bit and should work on other Debian/Ubuntu variants.

## Contact

Xiangyu Bu