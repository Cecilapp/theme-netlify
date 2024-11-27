# Netlify component theme

The _Netlify_ component theme for [Cecil](https://cecil.app) provides support of Netlify's [`_redirects`](https://docs.netlify.com/routing/redirects/) and [`_header`](https://docs.netlify.com/routing/headers/).

After installation and without any configuration, this component theme generate:

1. a [`_redirects`](./layouts/_default/page.netlify_redirects.twig) file containing HTML's redirections created by Cecil (automatic or created manually with the [`redirect`](https://cecil.app/documentation/content/#redirect) front matter variable)
2. a [`_headers`](./layouts/_default/page.netlify_headers.twig) file containg HTTP headers created by Cecil (generated from [`headers' configuration`](https://cecil.app/documentation/configuration/#headers))

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

### Add redirections

```yaml
netlify:
  redirects:
    - from: https://xxxxxx/*
      to: https://xxxxxx/:splat
      status: 301 # optional
      force: true # optional
```

Refer to [Netlify documentation](https://docs.netlify.com/routing/redirects/redirect-options/) for details.

#### Redirect home page to the user language version

```yaml
netlify:
  redirect_by_language: true # false by default
```

It generate the following redirect for each available language (other than the default):

```
/  /<language-code>/    302!    Language=<language-code>
```

_Example:_

```
/  /fr/    302!    Language=fr
```

> The language can be specified in the cookie `nf_lang`, so you can override the default behavior with JavaScript (in case of a language dropdown selector for example).

### Add headers

```yaml
headers:
  - path: <path> # Relative path, prefixed with a slash. Support "*" wildcard.
    headers:
      - key: <key>
        value: "<value>"
```

Document: <https://cecil.app/documentation/configuration/#headers>.
