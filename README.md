<h1 align="center">badge.mq</h1>

[shields.io](https://shields.io) badge generator implemented as an [mq](https://github.com/harehare/mq) module — build badge URLs and ready-to-paste Markdown image tags for READMEs without leaving your mq pipeline.

## Features

- Builds shields.io static badge URLs, with correct escaping of `-`, `_`, and spaces
- Renders a ready-to-paste Markdown badge, optionally wrapped in a link
- `status_color` maps common CI status words (`passing`, `failing`, `unknown`, …) to shields.io colors
- `build` / `license` / `version` convenience wrappers for the most common badges

## Installation

Copy `badge.mq` to your mq module directory, or place it anywhere and reference it with `-L`.

```sh
cp badge.mq ~/.local/mq/config/
```

### HTTP Import (no local installation needed)

If `mq` was built with the `http-import` feature, you can import directly from GitHub without any local setup:

```sh
mq -I raw 'import "github.com/harehare/badge.mq" | badge::license("MIT")' <<< "x"
```

Pin to a specific release with `@vX.Y.Z`:

```sh
mq -I raw 'import "github.com/harehare/badge.mq@v0.1.0" | ...'
```

## Usage

```sh
mq -L /path/to/modules -I raw \
  'import "badge" | badge::markdown("build", "passing", "brightgreen")' <<< "x"
```

If you copied it to the mq built-in module directory:

```sh
mq -I raw 'import "badge" | badge::markdown("build", "passing", "brightgreen")' <<< "x"
```

## API

| Function | Description |
|---|---|
| `url(label, message, color = "blue", style = None, logo = None)` | Builds a shields.io badge URL |
| `markdown(label, message, color = "blue", link = None, style = None, logo = None)` | Builds a Markdown `![...](...)` badge, optionally wrapped in `[...](link)` |
| `status_color(status)` | Maps a status word to a shields.io color (`passing`/`success`/`ok` -> `brightgreen`, `failing`/`error`/`fail` -> `red`, `unknown` -> `lightgrey`, else `yellow`) |
| `build(status, style = None)` | A build-status badge with color derived from `status_color` |
| `license(name, color = "blue", style = None)` | A `license: name` badge |
| `version(v, color = "blue", style = None)` | A `version: v` badge |

`color` accepts either a named shields.io color (`blue`, `brightgreen`, `red`, …) or a
hex code, with or without the leading `#`. `style` is one of shields.io's badge
styles (`flat`, `flat-square`, `plastic`, `for-the-badge`, `social`).

> **Note:** Only shields.io's own `-`/`_`/space escaping is applied — other characters
> that are special in URLs (`%`, `?`, `&`, `/`, …) are passed through as-is. This
> covers the vast majority of real-world labels/messages; if yours contains other
> reserved characters, percent-encode it yourself before calling `url`/`markdown`.

## Example

```sh
mq -I raw 'import "badge" | badge::url("coverage", "92%", "green", "flat-square")' <<< "x"
# => "https://img.shields.io/badge/coverage-92%-green?style=flat-square"

mq -I raw 'import "badge" | badge::markdown("license", "MIT", "blue", "https://opensource.org/licenses/MIT")' <<< "x"
# => "[![license: MIT](https://img.shields.io/badge/license-MIT-blue)](https://opensource.org/licenses/MIT)"

mq -I raw 'import "badge" | badge::build("passing")' <<< "x"
# => "![build: passing](https://img.shields.io/badge/build-passing-brightgreen)"
```

## Compatibility

Requires [mq](https://github.com/harehare/mq) v0.6 or later.

## License

MIT
