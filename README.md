bs
==

An environment manager for your shell.

Usage
-----

Place a `.env` file in your project with the environment variables you need.

For instance:

    $ cat .env
    RACK_ENV=development
    REDIS_URL=redis://127.0.0.1:6380

Then:

    $ bs rackup -p 8080

This will run `rackup` with your environment variables set.

If you want to drop to a new shell to work for a little while, just:

    $ bs

To restore the old environment, simply `Ctrl-D` and go back to the parent
shell.

Sometimes you'll want to modify the environment of your current shell without
creating a new one (for instance, if your init scripts modify the environment
and overwrite what `bs` is setting for you). That's easy to do:

    $ source bs

Or the more cryptic:

    $ . bs

Motivation
----------

We are long-time users of [`gs`][gs]. This small tool allows you to start a
shell (or run a command) within a specific gem set. And it does it beautifully
simply: by only setting a few environment variables which RubyGems will
honor. That's it.

Then there's [Foreman]. We use it to read our [Procfile] and start our
services.  But it also has the feature of loading a `.env` file and setting
environment variables before spawning your processes.

Now Foreman is a gem, so it should be installed inside the application's gem
set.  So, what if you need to run `irb` inside a bare shell?

    $ gs foreman run irb

We realized we could replace `gs` by setting the appropriate variables in our
`.env` file:

    GEM_HOME=$(pwd)/.gs
    GEM_PATH=$(pwd)/.gs:$(gem env path)
    PATH=$(pwd)/.gs/bin:$PATH

Now we had everything in our `.env` file and `foreman start` would pick it
up. But there are a few problems:

1. We don't use Foreman in production (at least not yet), so our deploy scripts
needed a simple way to also read `.env` without needing the whole of Foreman.

2. Foreman itself is a gem, so you would need to install it in your global gem
set.

3. Some people don't use Foreman at all.

We needed something simple, probably pure-shell, to read `.env` and either run
a given command or drop to a new shell with the variables set (similar to how
`bash` or `sh` behave).

Installation
------------

Place `bin/bs` somewhere in your `$PATH`.

With Homebrew:

    $ brew tap educabilia/tooling
    $ brew install bs

Support
-------

All of the features are tested on Bash and Zsh. We'll happily take patches
to make `bs` POSIX-compliant.

Acknowledgements
----------------

* [gs]
* [Foreman]
* [gst]


[gs]: https://github.com/soveran/gs
[Foreman]: http://ddollar.github.io/foreman
[Procfile]: http://ddollar.github.io/foreman/#PROCFILE
[gst]: https://github.com/tonchis/gst
