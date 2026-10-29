# Contributing

## Repository layout

```
asn/
├── README.md          English landing page — the charts, indexed
├── README.cn.md       中文 landing page
├── SKILL.md           How an AI agent or a script consumes this repo
├── CONTRIBUTING.md    this file
├── LICENSE            Apache-2.0 for the repo's own content
├── data/              the underlying data, as plain YAML
└── docs/              one directory per page; the source of asn.com/<name>/*
```

## Pages

Each page lives in `docs/<chapter>/<NN>-<slug>/` and holds:

| File | Purpose |
|---|---|
| `en.md` | English page |
| `cn.md` | 中文页 |
| `faq.yml` | one question/answer pair, consumed by the site's FAQ block |

- `docs/1-overview/01-about/` — Addresses, autonomous systems, and the five registries that hand them out / 地址、自治系统，以及发放它们的那五个机构
- `docs/2-registry/01-rir-allocation/` — Who hands out what, and what that shape means / 谁在分配什么，以及这种形状意味着什么
- `docs/2-registry/02-country-allocation/` — Address space and autonomous systems, country by country / 按国家看地址空间与自治系统

## The naming rule that the site depends on

The site builds `x-cmd.com/<name>/<slug>` from the **leaf directory name**, with
the numeric prefix stripped. So:

| Directory | Route |
|---|---|
| `docs/1-overview/01-about/` | the site index |
| `docs/2-registry/01-rir-allocation/` | `x-cmd.com/asn/rir-allocation` |

Two rules follow, and breaking either breaks the build:

1. **Leaf names must be unique across the whole repository.** They become
   top-level routes, so `02-x/` in one chapter and `03-x/` in another would
   collide.
2. **The first page of the first chapter is the index.** It is `01-*`, not
   `02-*`.

## The charts

Every figure is a plain `https://x-cmd.com/draw?type=…` URL, rendered to a
standalone SVG by [x-cmd/draw](https://github.com/x-cmd/draw). The URL is written
out in full in a ```text``` block, followed by an `<a href><img></a>` pair so
the page shows the picture and keeps the parameters visible and copyable.

**Never replace a draw URL with an image file.** The URL *is* the data: it is
machine-readable, editable, and the site re-renders it server-side.

## The numbers in `data/` come from the RIRs' own delegated statistics files, which are public registry exports. The delegated files themselves are the authoritative source; the YAML here is a derived snapshot so the site can be rebuilt without re-fetching five large files. Every figure carries an `as_of` date — re-derive them before publishing.

## Before you open a pull request

- [ ] `npx rumdl check .` passes (or the new finding is suppressed with a
      reason in `.rumdl.toml`)
- [ ] every new draw URL returns 200 — check it with
      `curl -s -o /dev/null -w '%{http_code}' '<url>'`
- [ ] the page exists in **both** `en.md` and `cn.md`
- [ ] `faq.yml` has an entry for the new page
- [ ] the numbers are re-derived from the primary source, not copied from the
      previous page
