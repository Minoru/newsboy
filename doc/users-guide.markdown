User's Guide
============

**Note:** Newsboy handles RSS and Atom feeds equally good, but for brevity I'll
refer to both formats collectively as "RSS".

# What Newsboy does

Newsboy is meant to be run on a server, a machine that's on 24/7 and is
accessible over the network. You configure your newsreader to use Newsboy as
HTTP proxy. When your reader requests a feed, Newsboy will proxy the response
and then start following that feed. Next time your newsreader request that feed,
Newsboy will deliver all the items it has seen in that feed since the last
request. This way, you can forgo running your newsreader for weeks on end, but
still get all news items that were issued in the meantime.

# But what about HTTPS?

[HTTPS](https://en.wikipedia.org/wiki/HTTPS) is meant to secure communications
between client and server, ensuring no-one is spying on your connection, but in
doing so it makes life harder for legitimate applications that need to read and
modify the exchanges.

There are a couple workarounds for that: 1) fetch feeds over HTTP; 2) disable
certificate verification in the newsreader, letting it accept self-signed
certificates produced by Newsboy; 3) add Newsboy's self-signed root certificate
to the newsreader's "CA store", making the newsreader trust all the
certificates that Newsboy produces and signs. Using HTTP quickly becomes
impractical as sites are eagerly adopting HTTPS, including HTTP-to-HTTPS
redirects.

This leaves us with options 2) and 3). Upon closer inspection, it turns out
those aren't conflicting: we can pick 3 as a stronger one, and if a user can't
add CA root to the store, they can try option 2.

# `status.rss`/`status.atom` feed

Newsboy is meant to be as transparent as possible: apart from initial setup,
users shouldn't think about it. But there are situations when Newsboy has to
notify the user of something, for example:

* an error has encountered while fetching a feed;
* an error went away;
* and more.

Being RSS-related software, it's obvious how Newsboy provides such notices to
the user: via an RSS feed! You can find it at the same domain where you put
your Newsboy installation; the name of the feed is `status.rss` or
`status.atom` (pick the format you prefer).

Newsboy tries to keep notices to the minimum. If an error occurs, but goes away
before your newsreader had a chance to fetch the corresponding notice, you won't
get any at all. But if you've seen a notice about an error, Newsboy will make
sure to notify you when it's gone.
