## Tools

Tools can be found under the [bin/](bin/) directory. All tools run on macOS and require
a Homebrew installation.

### `brew-mirror`

`brew-mirror` performs a mirror of all (supported) Homebrew source packages.

It takes a few different options and isn't finished yet; read the source code.

### `brew-offline-install`

`brew-offline-install` takes the name of a formula to install from the local mirror.

It takes normal `brew install` options, but will stop you if you try to pass an option
that will circumvent the mirror (e.g., asking for a `HEAD` or non-stable spec).

### `brew-offline-curl`

`brew-offline-curl` is the `curl` shim called by `brew-offline-install` when making "normal"
(i.e., HTTP(S)) requests. It rewrites the requested URL internally and feeds the rewritten URL to
the real `curl`.

You shouldn't (need to) call it directly.

### `brew-offline-git`

`brew-offline-git` is the `git` shim called by `brew-offline-install` when making Git repository
(i.e. `git clone`) requests. Like `brew-offline-curl`, it rewrites the requested URL
internally and feeds the rewritten URL to the real `git`.

You shouldn't (need to) call it directly.

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
