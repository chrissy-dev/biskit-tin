# Biskit Tin 

Named after the actual biscuit tin(s) I had in my upstairs cupboard in the 90s as a wee boy full of loose family photos, and my family dog, Biskit (2000-2014). 

## What is this? 

A command-line tool that generates a static, browsable HTML archive from a folder of images. It reads from the filesystem, writes only generated artefacts, and never modifies original files. Built for longevity over cleverness -- the kind of tool a non-technical family member can open in a browser in twenty years without knowing what a server is.

## Core Principles

- The filesystem is the database. Folder structure is the only organisation.
- Never modify, rename, or move original files. Ever.
- Generated artefacts are the only output. Delete them and nothing of yours is lost.
- No server. No database. No internet connection. No dependencies on the user's machine.
- A complete archive is a folder you can put on a hard drive and forget about for twenty years.
- If it doesn't work with JavaScript disabled, it's broken.

## Folder Structure

Biskit Tin expects and respects the following structure. It imposes nothing.

```
📂 Photos/
├── 📂 2024-Scotland-Trip/
│    ├── 🖼 image1.jpg
│    ├── 🖼 image2.png
│    ├── 📄 meta.md  (optional)
│    ├── 📂 .thumbs/ (generated)
│    └── 📜 index.html (generated)
│
├── 📂 2023-Italy/
│    ├── 🖼 photo.jpg
│    ├── 📄 meta.md (optional)
│    ├── 📂 .thumbs/ (generated)
│    └── 📜 index.html (generated)
│
├── 📜 style.css (generated)
└── 📜 index.html (generated)
```

- Folders are collections. Nesting is supported but not required.
- `meta.md` is optional plain text or markdown. Biskit Tin reads it, never writes it.
- `.thumbs/` contains only generated thumbnails. Safe to delete, will be regenerated.
- Any folder without images is ignored.

## Tins

A tin is a single managed image archive - a root folder that Biskit Tin knows about. You can have as many tins as you want, wherever you want. A tin on an external family hard drive and a tin for your photography work are completely independent.

Biskit Tin maintains a local registry of tins so you can switch between them by name rather than path.

- `biskit tin add <name> <path>` -- register a new tin
- `biskit tin remove <name>` -- unregister a tin (does not delete anything)
- `biskit tin list` -- list all registered tins
- `biskit tin use <name>` -- set the active tin

All other commands operate on the active tin unless a `--tin <name>` flag is passed.