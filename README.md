# Muon

Muon is an XML document standard to store users' subscriptions to Atom, RSS and other feeds. It's supposed to provide flexible, feature-rich, future-proof and strictly defined standards better than classic OPML (well, I think this might be nice try).

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
        <feed enabled="true" source="https://news.ycombinator.com/rss" output="hackernews.feed" />
        <feed enabled="true" source="https://blog.mozilla.org/feed" output="mozilla.feed" />
        <feed enabled="true" source="https://www.reddit.com/r/linux/.rss" output="reddit-linux.feed" />
        <feed enabled="false" source="https://github.com/curl/curl/commits.atom" output="github-curl.feed" />
      </feeds>
    </body>
  </muon>
```

## Why not OPML?

OMPL is just *general purpose* outliner format and as for me it's *strange* to use it specifically to feeds. It provides camelCased namespace with practically useless elements like `<vertScrollState>`. One can't define alternate URLs, set credentials or use filtering rules to exclude unwanted items in feeds. OPML specs is too short, ambiguous and no one can guarantee that your OPML will be parsed correctly by new software.

## Specification

### Base Elemets

Valid Muon document consists of `<muon>` root element and its two child elements, `<head>` and `<body>`:

```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <muon version="1.0">
    <head> ... </head>
    <body> ... </body>
  </muon>
```

### Head Element

Head element currently contains only document metadata key-value pairs for information such as creation date:

```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <muon version="1.0">
    <head>
      <meta key="..." value="..." />
    </head>
    <body> ... </body>
  </muon>
```

Each `<meta>` element must have `key` and `value` attributes. There is no strict definition for `key` names — however it's recommended to use following keys:

  - `created` — document creation time and date. Value should be specified as in ISO 8601 or RFC 3339 (like `YYYY-MM-DDTHH:MM:SS` or shortened derivatives). Meta element with `key="created"` should be specified only once.
  - `modified` — document modification time, formatted as defined for `created` key above.
  - `creator` — creator (author) information. May be the name, nickname, email or other text in a free form
  - `comment` — optional text in a free form. This key can be used to tag document by external software

Each meta element is optional. Elements with keys `modified`, `creator` and `comment` may be specified multiple times.

```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <muon version="1.0">
    <head>
      <meta key="created" value="2018-04-01T10:00:53+00:00" />
      <meta key="modified" value="2018-05-01T11:26:02+00:00" />
      <meta key="modified" value="2018-12-01T23:47:16+00:00" />
      <meta key="creator" value="John Doe &lt;john@example.org&gt;" />
      <meta key="comment" value="Sample Muon file" />
      <meta key="comment" value="Created by Muon-O-Matic application" />
    </head>
    <body> ... </body>
  </muon>
```

### Body Element

Body element currently includes only `<feeds>` element which stores `<feed` child elements:

```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <muon version="1.0">
    <head> ... </head>
    <body>
      <feeds>
        <feed ... />
        <feed ... />
        <feed ... />
      </feeds>
    </body>
  </muon>
```

Each feed element should include the following attributes:

  - `enabled` — specifies external software behavior. If `enabled="true", then software must retrieve items for this feed, and it must do nothing if `enabled="false". This attribute may not be specified, in such case it assumed as `enabled="true"`.
  - `source` — URL for original feeds list, probably HTTP(s) address, e.g. `source="http://example.org/rss"`.
  - `output` — When present, it indicates that external software should save retrieved feed items to file specified in `output` attribute value. File name should be relative to software working directory which should be specified in external software settings. If `output` is not specified, then feed items should be saved in other way, e.g. by using cache directory.

```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <muon version="1.0">
    <head> ... </head>
    <body>
      <feeds>
        <feed enabled="true" source="http://example.org/rss" output="example.feed" />
      </feeds>
    </body>
  </muon>
```

In example above, feed from http://example.org/rss will be saved to file named `example.feed`. This file will be located in external software working directory, e.g. ~/Downloads/Feeds.

## TODO

  - [x] Basic definition
  - [ ] DTD or similar schema
  - [ ] Credentials support
  - [ ] Filtering rules
  - [ ] Scrapping rules
