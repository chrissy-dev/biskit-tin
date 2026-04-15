# Biskit Tin 

Named after the actual biscuit tin(s) I had in my upstairs cupboard in the 90s as a wee boy full of loose family photos, and my family dog, Biskit (2000-2014). 

## What is this? 

A command-line tool that generates a static, browsable HTML archive from a folder of images. It reads from the filesystem, writes only generated artefacts, and never modifies original files. Built for longevity over cleverness the kind of tool a non-technical family member can open in a browser in twenty years without knowing what a server is.

## Core Principles

- The filesystem is the database. Folder structure is the only organisation.
- Never modify, rename, or move original files. Ever.
- Generated artefacts are the only output. Delete them and nothing of yours is lost.
- No server. No database. No internet connection. No dependencies on the user's machine.
- A complete archive is a folder you can put on a hard drive and forget about for twenty years.
- If it doesn't work with JavaScript disabled, it's broken.