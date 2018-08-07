offlinebrew
===========

Offlinebrew is a collection of tools for running
[Homebrew](https://brew.sh) in offline environments.

## Overview

Homebrew requires an active internet connection for most actions. Offlinebrew
circumvents this requirement by pre-fetching the files used by common
`brew` invocations, and redirecting Homebrew's cache to avoid network access.

## Status

Currently, offlinebrew works by faking the Homebrew cache. Since Homebrew both
reads from and writes to the cache during normal operation, every user must have their
own copy of the cache. This is fine when working individually, but makes it currently impossible
to just drop the cache on a network share for mulitple people to use.

The easiest workaround for this is probably to use a
[union mount](https://en.wikipedia.org/wiki/Union_mount) with the initial cache as read-only
and the user's cache operations redirected to a writable scratch folder.
[`bindfs`](https://bindfs.org/) can probably enable this.

## Tools

Tools can be found under the [bin/](bin/) directory. All tools run on macOS and require
a Homebrew installation.

### `dump_formulae`

`dump_formulae` dumps all stable source packages in
[`homebrew-core`](https://github.com/Homebrew/homebrew-core), including all external resources
and external patches.

#### Usage

The following invocation instructs `dump_formulae` to download all stable source packages
to a user-specified directory. The directory must already exist.

```bash
# Note: dump_formulae runs under `brew ruby` to get access to Homebrew's Ruby API.
$ brew ruby dump_formulae -d /path/to/cache
```

`-s`, `--sleep` can also be used to specify a sleep duration between formulae downloads:

```bash
$ brew ruby dump_formulae -d /path/to/cache -s 0.5
```

### `brew-offline`

`brew-offline` runs `brew` with a redirected cache and other tweaks.

#### Usage

```bash
$ brew-offline install gcc
```

