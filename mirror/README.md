## Tools

Tools can be found under the [bin/](bin/) directory. All tools run on macOS and require
a Homebrew installation.

### `brew-mirror`

`brew-mirror` performs a mirror of all (supported) Homebrew source packages.

The simplest way to run `brew-mirror` is to feed it an output directory:

```bash
$ mkdir /tmp/brew-mirror
$ brew ruby ./bin/brew-mirror -d /tmp/brew-mirror
```

A complete source tree mirror requires about 60GB of space and takes about 8 hours.
You can cancel and resume it, although `git` repositories are always re-fetched and re-prepped.

Once complete, running the mirror is as simple as serving the mirror directory over HTTP:

```bash
$ cd /tmp/brew-mirror
$ pythom -m SimpleHTTPServer
```

`brew-mirror` makes fixups to the `git` repositories that it fetches to make this possible.

### `brew-mirror-prune`

`brew-mirror-prune` is an *optional* script for managing the size of the mirror. It'll remove
any files or directories (i.e., `git` repositories) that aren't currently being advertised
as available by the mirror.

This is mostly useful for removing the occasional duplicate of large `git` repositories.

You should pass it the same directory as `brew-mirror`:

```bash
$ brew-mirror-prune -d /tmp/brew-mirror
```

### `brew-offline-install`

`brew-offline-install` takes the name of a formula to install from the local mirror.

It takes normal `brew install` options, but will stop you if you try to pass an option
that will circumvent the mirror (e.g., asking for a `HEAD` or non-stable spec).

### `brew-offline-curl`

`brew-offline-curl` is the `curl` shim called by `brew-offline-install` when making "normal"
(i.e., HTTP(S)) requests. It rewrites the requested URL internally and feeds the rewritten URL to
the real `curl`.

You shouldn't (need to) call it directly, but it *does* need to go into your `$PATH`.

### `brew-offline-git`

`brew-offline-git` is the `git` shim called by `brew-offline-install` when making Git repository
(i.e. `git clone`) requests. Like `brew-offline-curl`, it rewrites the requested URL
internally and feeds the rewritten URL to the real `git`.

You shouldn't (need to) call it directly, but it *does* need to go into your `$PATH`.

## Configuration

### Client-side

Clients should only need one configuration file: `~/.offlinebrew/config.json`.

Additionally, clients only need to worry about one key in *config.json*:

```json
{
    "baseurl": "http://localhost:8000"
}
```

`brew-offline-install` will take care of adding additional information to *config.json*.

### Mirror-side

`brew-mirror` writes two files to the mirror directory: *config.json* and *urlmap.json*.

*config.json* contains various configuration settings read by `brew-offline-install` and the
`curl`/`git` shims. You shouldn't need to modify it by hand.

*urlmap.json* contains a mapping of original resource URLs
(e.g., `https://example.com/foobar-4.0.tar.gz`) to unique identifiers + extensions
(e.g., `f2c1e86ca0a404ff281631bdc8377638992744b175afb806e25871a24a934e07.tar.gz`). These
identifiers + extensions are expected to exist under the `baseurl` key in *config.json*.
You shouldn't need to modify *urlmap.json* by hand.

As an example, if `baseurl` is `http://192.168.1.5:8080`, then *urlmap.json* tells the individual
shims that `https://example.com/foobar-4.0.tar.gz` is actually
`http://192.168.1.5:8080/f2c1e86ca0a404ff281631bdc8377638992744b175afb806e25871a24a934e07.tar.gz`.
