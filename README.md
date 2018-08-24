offlinebrew
===========

Offlinebrew is a collection of tools for running
[Homebrew](https://brew.sh) in offline environments.

## Overview

Homebrew requires an active internet connection for most actions. Offlinebrew
circumvents this requirement.

## Status

The [cache_based/](cache_based/) directory contains an initial approach, based on
spoofing `HOMEBREW_CACHE`.

The [mirror/](mirror/) directory will contains a more complete approach, based on
URL rewriting (enabling redirection to a local host).

