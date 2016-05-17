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

# Note on HTTPS URLs

[HTTPS](https://en.wikipedia.org/wiki/HTTPS) is meant to secure communications
between client and server, ensuring no-one is spying on your connection, but in
doing so it makes life harder for legitimate applications that need to read and
modify the exchanges.

Unfortunately, all workarounds for that are inconvenient to some extent: some
require you newsreader to be able to accept self-signed certificates without
much fuss, other put a bit of burden on the user. It's silly to limit our
userbase to people who use configurable enough newsreaders, so Newsboy is forced
to make users do little work every time they subscribe to a feed.

Specifically, Newsboy requires you to only use HTTP schema in feed URLs, i.e.
start them with `http://` and not `https://`. This way, your newsreader won't
require HTTPS connection and will thus allow Newsboy to do its job.

But what if you forget to do that? Don't you worry! Newsboy will notice that
you're requesting a feed and will fetch it for you, but it won't be able to
serve it for reasons mentioned above. When you finally notice that you have
HTTPS URL and edit it, you'll get all the items you would've got if you used
HTTP URL from the start. Newsboy got your back!

And remember: using HTTP URLs for feeds doesn't mean your communications are
now in clear! Your newsreader and Newsboy communicate over HTTPS themselves,
and Newsboy makes sure to fetch your feeds over HTTPS whenever appropriate. In
the next section, we will describe how you can control the latter feature.

# `newsboy_fetch_over_https=(on|off)`

As mentioned in the previous section, users should only use `http://` URLs in
their newsreaders. But that doesn't mean Newsboy will necessarily use HTTP to
fetch them!

Here's what happens by default: on first fetch, Newsboy will request the feed
both over HTTP and HTTPS. If only one request succeeds, the corresponding
protocol will be used hereafter. If both requests succeed, HTTPS will be used,
but a warning will be issued if feeds are different (see "`status.rss` feed"
section for details on warnings, and "How feeds are compared" section for
details on feed comparison procedure.) This mode is called an "auto".

If you want specific feed to always be fetched over HTTPS, you can append
`?hewsboy_fetch_over_https=on` to its URL (if the URL already has question mark
in it, use ampersand instead, i.e. append `&newsboy_fetch_over_https=on`).

If you want specific feed to always be fetched over HTTP, append
`?newsboy_fetch_over_https=off` (or `&newsboy_fetch_over_https=off` if the URL
already contains a question mark.)

Note that the server can respond with a permanent redirect (HTTP status code
302), which might use a schema different from the one you requested. In that
case, Newsboy will ignore `newsboy_fetch_over_https` parameter and use the
address supplied by the server.

HTTPS or HTTP is enforced as long as `newsboy_fetch_over_https` parameter is
present. If you remove it, this will put the feed back into "auto" mode.

# How feeds are compared

For two feeds to be considered the same, all of the following conditions must
hold:

1. Have the same title.
2. Contain the same number of items.
3. Each item from one feed have a corresponding item in another feed, having the
   same ID, title, date of publication, last update date, and body.

These can be summed up intuitively as "have the same content".

# `status.rss`/`status.atom` feed

Newsboy is meant to be as transparent as possible: apart from initial setup and
continuing effort to use `http://` scheme when adding a URL, users shouldn't
think about it. But there are situations when Newsboy has to notify user of
something, for example:

* an error was encountered while fetching a feed;
* an error went away;
* user forgot to change URL schema to `http://`;
* and more.

Being RSS-related software, it's obvious how Newsboy provides such notices to
the user: via RSS feed! You can find it at the same domain where you put your
Newsboy installation; the name of the feed is `status.rss` or `status.atom`
(pick the format you prefer).

Newsboy tries to keep notices to the minimum. If an error occurs, but goes away
before your newsreader had a chance to fetch the corresponding notice, you won't
get any at all. But if you've seen a notice about an error, Newsboy will make
sure to notify you when it's gone.
