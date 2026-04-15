# Biskit Tin

A command-line tool that generates a static, browsable HTML archive from a folder of images.

Named after the actual biscuit tin full of loose family photos I had as a wee boy in the 90s, and my family dog, Biskit (2000–2014).

## What it does

Point it at a folder of images. It generates an `index.html` per folder, thumbnails, and a shared stylesheet. Open the result in a browser. No server. No internet connection. No dependencies on whoever opens it.

Built for longevity over cleverness — the kind of thing a non-technical family member can open in twenty years without knowing what a server is.

## Principles

- The filesystem is the database. Folder structure is the only organisation.
- Never modify, rename, or move original files. Ever.
- Generated artefacts are the only output. Delete them and nothing of yours is lost.
- If it doesn't work with JavaScript disabled, it's broken.

## Usage

```sh
biskit tin add family /Volumes/Photos/Family   # register a tin
biskit tin use family                          # set it as active

biskit rebuild                                 # generate everything from scratch
biskit refresh                                 # add what's missing, skip the rest
biskit status                                  # check archive health without changing anything
```

## Status

Early development. Spec-first, built in Go.
