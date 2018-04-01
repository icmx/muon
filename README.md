# Muon

Muon is an XML document standard to store and synchronize user's subscriptions to Atom, RSS and other feeds. It's supposed to provide flexible, feature-rich, future-proof and strictly defined standards better than classic OPML (well, I think this might be nice try).

## Quick Example

```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <muon version="1.0">
    <head>
      <meta key="created" value="2018-04-01T10:00:53+00:00" />
      <meta key="comment" value="Sample Muon file" />
    </head>
    <body>
      <feeds>
        <feed enabled="true" source="https://news.ycombinator.com/rss" result="hackernews.feed" />
        <feed enabled="true" source="https://blog.mozilla.org/feed" result="mozilla.feed" />
        <feed enabled="true" source="https://www.reddit.com/r/linux/.rss" result="reddit-linux.feed" />
        <feed enabled="false" source="https://github.com/curl/curl/commits.atom" result="github-curl.feed" />
      </feeds>
    </body>
  </muon>
```

## Why not OPML?

OMPL is just *general purpose* outliner format and as for me it's *strange* to use it specifically to feeds. It provides camelCased namespace with practically useless elements like `<vertScrollState>`. One can't define alternate URLs, set credentials or use filtering rules to exclude unwanted items in feeds. OPML specs is too short, ambiguous and no one can guarantee that your OPML will be parsed correctly by new software.

## Links

  - One can use [Jonesy](https://github.com/icmx/jonesy) — currently the single software that works with Muon files.

## TODO

  - [ ] Basic definition
  - [ ] DTD or similar schema
  - [ ] Credentials support
  - [ ] Filtering rules
  - [ ] Scrapping rules
