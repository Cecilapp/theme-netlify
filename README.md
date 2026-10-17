# Netlify component theme

The _Netlify_ component theme for [Cecil](https://cecil.app) provides support of Netlify's [`_redirects`](https://docs.netlify.com/routing/redirects/) and [`_headers`](https://docs.netlify.com/routing/headers/) files.

After installation and without any configuration, this component theme generates:

1. a [`_redirects`](./layouts/_default/page.netlify_redirects.twig) file containing:
   - HTML redirections created by Cecil, in all languages (automatic or created manually with the [`redirect`](https://cecil.app/documentation/content/#redirect) front matter variable)
   - a localized 404 error page for each language (multilingual sites only)
2. a [`_headers`](./layouts/_default/page.netlify_headers.twig) file containing default HTTP headers (security and cache) and the HTTP headers defined in the [`server.headers` configuration](https://cecil.app/documentation/configuration/#headers)

## Installation

```bash
composer require cecil/theme-netlify
```

> Or [download the latest archive](https://github.com/Cecilapp/theme-netlify/releases/latest/) and uncompress its content in `themes/netlify`.

## Usage

Add `netlify` in the `theme` section of your site configuration:

```yaml
theme:
  - netlify
```

The `_redirects` and `_headers` files are generated at the root of the output directory (`_site/` by default), where Netlify expects them.

### Configuration

Options and their default values:

```yaml
netlify:
  redirects: []               # custom redirect rules
  redirect_by_language: false # redirect home page to the user language version
  localized_404: true         # localized 404 error pages
  default_headers: [...]      # default HTTP headers (`false` to disable)
```

### Add redirections

```yaml
netlify:
  redirects:
    - from: /old-path/*
      to: /new-path/:splat
      status: 301 # optional (301 by default if `force` is set)
      force: true # optional
```

Refer to [Netlify documentation](https://docs.netlify.com/routing/redirects/redirect-options/) for details.

#### Redirect home page to the user language version

```yaml
netlify:
  redirect_by_language: true # false by default
```

It generates the following redirect for each available language (other than the default):

```
/  /<language-code>/    302!    Language=<language-code>
```

_Example:_

```
/  /fr/    302!    Language=fr
```

> The language can be specified in the cookie `nf_lang`, so you can override the default behavior with JavaScript (in case of a language dropdown selector for example).

#### Localized 404 error pages

On a multilingual site, Netlify serves the 404 page of the default language (`/404.html`) for every missing page. This theme generates a rule for each other language, so that the 404 page is displayed in the right language:

```
/fr/*    /fr/404.html    404
```

A rule is generated only if the 404 page exists in that language. To disable it:

```yaml
netlify:
  localized_404: false
```

### Add headers

```yaml
server:
  headers:
    - path: <path> # Relative path, prefixed with a slash. Support "*" wildcard.
      headers:
        - key: <key>
          value: "<value>"
```

Documentation: <https://cecil.app/documentation/configuration/#headers>.

Your headers are added after the default headers of the theme.

#### Default headers

The theme provides the following [default headers](./config.yml):

| Path | Headers |
| --- | --- |
| `/*` | `X-Frame-Options`, `X-Content-Type-Options`, `Strict-Transport-Security` |
| `/favicon*`, `/logo*`, `/icon*`, `/assets/*`, `/images/*`, `/videos/*`, `/styles/*`, `/css/*`, `/*.css`, `/fonts/*` | `Cache-Control: public, max-age=31536000` (1 year) |
| `/javascript/*`, `/js/*`, `/*.js` | `Cache-Control: private, max-age=31536000` (1 year) |
| `/serviceworker.js`, `/manifest.webmanifest` | `Cache-Control: no-cache` |
| `/admin/*`, `/.netlify/*` | `Cache-Control: no-cache, no-store, must-revalidate`, permissive `Content-Security-Policy` |

> [!IMPORTANT]
> Files with a long cache lifetime should have a fingerprint in their name (see [`assets.fingerprint`](https://cecil.app/documentation/configuration/#assets)), otherwise visitors may not receive their updates.

To replace the default headers, copy them from the theme [`config.yml`](./config.yml) into the `netlify.default_headers` section of your site configuration, then edit them. To disable them:

```yaml
netlify:
  default_headers: false
```

> When a path matches several rules, Netlify applies the headers of all matching rules.

## License

_Netlify_ is a free software distributed under the terms of the MIT license.

© [Arnaud Ligny](https://arnaudligny.fr)
