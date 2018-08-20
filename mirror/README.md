## Tools

Tools can be found under the [bin/](bin/) directory. All tools run on macOS and require
a Homebrew installation.

### `brew mirror`

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

`brew-offline-git` doesn't exist yet, but will be the `git` shim called by `brew-offline-install`
when making Git repository requests. Like `brew-offline-curl`, it rewrites the requested URL
internally and feeds the rewritten URL to the real `git`.

You shouldn't (need to) call it directly.
