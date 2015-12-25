newsboy
=======

Newsboy is an Atom/RSS feeds fetcher (**not** a reader!) It acts as a proxy for
your newsreader, and will continue fetching all the feeds you've requested
lately, caching them to deliver when you next open your reader.

Installation
------------

At the moment, the only way to install newsboy is to build it from the source.
The only requirement is a [`stack`](http://haskellstack.org) utility.

Run `stack install` in the directory with `newsboy`'s source code, and `newsboy`
executable will be installed into `~/.local/bin` (you can run `stack path
--local-bin-path` to check this path).

Usage
-----

This section is quick setup guide. Please refer to User's Guide for details.
