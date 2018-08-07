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

