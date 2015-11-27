newsboy
=======

Newsboy is an Atom/RSS feeds fetcher, not a reader. All it does is fetch your
feeds and deliver their contents using any of a number of APIs (see the list at
the end of this document.)

Installation
------------

At the moment, the only way to install newsboy is to build it from the source.
The only requirement for that is a [`stack`](http://haskellstack.org) utility.

Run `stack install` in the directory with `newsboy`'s source code, and `newsboy`
exacutable will be installed into `~/.local/bin` (you can run `stack path
--local-bin-path` to check this path).

Usage
-----

This section is quick setup guide. Please refer to User's Guide for details.

Supported APIs
--------------

Newsboy can provide you with access to your news items via the following APIs:

* FeedHQ / Google Reader
* Tiny Tiny RSS

