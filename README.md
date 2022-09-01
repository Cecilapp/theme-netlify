# Netlify component theme

The _Netlify_ component theme for [Cecil](https://cecil.app) provides support of Netlify's [`_redirects`](https://docs.netlify.com/routing/redirects/) and [`_header`](https://docs.netlify.com/routing/headers/).

## Installation

```bash
composer require cecil/theme-netlify
```

> Or [download the latest archive](https://github.com/Cecilapp/theme-netlify/releases/latest/) and uncompress its content in `themes/netlify`.

## Usage

Add `netlify` in the `theme` section of your `config.yml`:

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

### Enable redirect by language

```yaml
netlify:
  redirect_by_language: true # false by default
```

It generate the following redirect for each available language other than the default:

```
/  /<language-code>/    302!    Language=<language-code>
```

_Example:_

```
/  /fr/    302!    Language=fr
```

> The language can be specified in the cookie `nf_lang`, so you can override the default behavior with JavaScript (in case of a language dropdown selector for example).
