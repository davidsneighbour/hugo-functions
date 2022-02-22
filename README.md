<!--- CARD BEGIN --->

![@dnb-org/dnb-hugo-functions](.github/github-card-dark.png#gh-dark-mode-only)
![@dnb-org/dnb-hugo-functions](.github/github-card-light.png#gh-light-mode-only)

<!--- CARD END --->

# DNB GoHugo Component / Functions

A Hugo theme component with helper functions for other projects.

<!--- THINGSTOKNOW BEGIN --->

## Some things you need to know

These are notes about conventions in this README.md. You might want to make yourself acquainted with them if this is your first visit.

<details>

<summary>:heavy_exclamation_mark: A note about proper configuration formatting. Click to expand!</summary>

The following documentation will refer to all configuration parameters in TOML format and with the assumption of a configuration file for your project at `/config.toml`. There are various formats of configurations (TOML/YAML/JSON) and multiple locations your configuration can reside (config file or config directory). Note that in the case of a config directory the section headers of all samples need to have the respective section title removed. So `[params.dnb.something]` will become `[dnb.something]` if the configuration is done in the file `/config/$CONFIGNAME/params.toml`.

</details>
<!--- THINGSTOKNOW END --->

<!--- INSTALLUPDATE BEGIN --->

## Installing

First enable modules in your own repository:

```bash
hugo mod init github.com/username/reponame
```

Then add this module to your required modules in config.toml.

```toml
[module]

[[module.imports]]
path = "github.com/dnb-org/dnb-hugo-functions"

```

The next time you run `hugo` it will download the latest version of the module.

## Updating

```shell
# update this module
hugo mod get -u github.com/dnb-org/dnb-hugo-functions
# update all modules
hugo mod get -u ./...
```
<!--- INSTALLUPDATE END --->

### Working principle

While being named `functions` this component adds merely partials that return values. In these partials calculations are done, so we might un-nerdily call them functions.

### Available Functions

- [getRandomString](#getrandomstring)
- [getReadingTime](#getreadingtime)
- [getYear](#getyear)
- [isCJK](#iscjk)
- [printCommentHeader](#printcommentheader)
- [truncate](#truncate)

#### getRandomString

_since 1.0.0_

To be written.

#### getReadingTime

_since 1.0.0_

```golang
{{- $readingtime := partial "func/getReadingTime" . -}}
```

- Context: a dictionary containing at least `.Content` to be counted.
- Returns: a dict with `minutes` (int), `seconds` (int), `words` (int)

**Configuration**

```toml
[readingtime]
wordsperminute = 220.0
minutesandseconds = true
```

Configuration options in `data/dnb/functions/config.toml`:

- wordsperminute (default 220.0) - Words that are read per minute. This must be a float value, not an integer! (no quotation marks and at least ending in .0 to typecast it as float)
- minutesandseconds (default true) - should the function return minutes and seconds or only minutes (the latter will be rounded up for the seconds in addition)

#### getYear

_since 1.0.0_

Returns the current year. Use it for instance to display the current year in your copyright byline.

```golang
{{- partialCached "func/getYear" . . -}}
```

#### isCJK

_since 1.0.1_

Checks if the string submitted contains characters of the [CJK Unified Ideographs](https://en.wikipedia.org/wiki/CJK_Unified_Ideographs) (or any configured) character range, i.e. if the string is Chinese, Japanese, or Korean.

```golang
{{- partial "func/isCJK.html" "a string" -}}
{{- partial "func/isCJK.html" (dict "content" "a string" "against" "cjk") -}}
```

Either submit a string or a dictionary containing the following parameters:

- `content` - a string to test against
- `against` - the range to test the string.
  - `cjk` (default) for Chinese, Japanese or Korean
  - `th` for Thai
  - `mm` for Myanmar (or Burmese)
  - `mn` for Mongolian

The partial checks against a configured [Unicode Block](https://en.wikipedia.org/wiki/Unicode_block), the results with special characters or encoding might vary.

#### printCommentHeader

_since 1.0.5_

```golang
{{ partial "func/printCommentHeader.html" "Start of section x" }}
```

will print

```html
<!--############################################################################
    # Start of section x
    ############################################################################-->
```

This function will return nothing if the environment is set to production.

#### truncate

_since 1.0.2_, see [GoHugo-Discourse thread](https://discourse.gohugo.io/t/create-description-from-summary/36676), that led to this function.

```golang
{{ range seq 1 200 }}
  {{ $truncated := partial "func/truncate.html"
      (dict
        "content" $.Summary
        "maxLength" .
      ) }}
  <pre>
    {{ $truncated }}
    (max = {{ . }}
    actual = {{ strings.RuneCount $truncated }} )
  </pre>
{{ end }}
```

Truncates a submitted string by cutting at word borders. If the truncation occurs inside of a sentence then an ellipsis is added. If the truncation occurs at a sentence border (.?!) no ellipsis is added.

**_TODO:_** If the string is within the CJK range then the string is truncated without regards to word or sentence borders and always has an ellipsis added if the string is longer than `maxLength`.

Expects a dictionary with the following content:

- `.content` - (required) content to truncate
- `.maxlength` - (optional) length at which to truncate, defaults to 150 characters

Notes:

- `len` returns the _byte_ length of a string. Whatever defines a character might be something else in some languages.

<!--- COMPONENTS BEGIN --->

## Other [GoHugo](https://gohugo.io/) components by [@dnb-org](https://github.com/dnb-org/)

<!-- prettier-ignore -->
| Component | Description |
| :--- | :--- |
| [dnb-hugo-auditor](https://github.com/dnb-org/dnb-hugo-auditor) | |
| [dnb-hugo-debug](https://github.com/dnb-org/dnb-hugo-debug) :mage_man: | Debug EVERYTHING in GoHugo. |
| [dnb-hugo-errors](https://github.com/dnb-org/dnb-hugo-errors) | A Hugo module that adds more versatile error pages to a site. |
| [dnb-hugo-feeds](https://github.com/dnb-org/dnb-hugo-feeds) | Implements various configurable feed formats. |
| [dnb-hugo-functions](https://github.com/dnb-org/dnb-hugo-functions) | A Hugo theme component with helper functions for other projects. |
| [dnb-hugo-giscus](https://github.com/dnb-org/dnb-hugo-giscus) | The Giscus comment system layout for GoHugo. |
| [dnb-hugo-head](https://github.com/dnb-org/dnb-hugo-head) | A GoHugo theme component that solves the old question of "What tags belong into the `<head>` tag of my website?" |
| [dnb-hugo-hooks](https://github.com/dnb-org/dnb-hugo-hooks) | Hooks for GoHugo layouts. An easy way for theme developers to let users add to their themes.  |
| [dnb-hugo-humans](https://github.com/dnb-org/dnb-hugo-humans) | Your site is made by humans. Humans.txt is naming them. |
| [dnb-hugo-icons](https://github.com/dnb-org/dnb-hugo-icons) | Icons for your Hugo website. |
| [dnb-hugo-internals](https://github.com/dnb-org/dnb-hugo-internals) | Better internal templates for GoHugo |
| [dnb-hugo-netlification](https://github.com/dnb-org/dnb-hugo-netlification) | a collection of tools that optimize your site on Netlify |
| [dnb-hugo-opensearch](https://github.com/dnb-org/dnb-hugo-opensearch) | configuration for Open Search |
| [dnb-hugo-pictures](https://github.com/dnb-org/dnb-hugo-pictures) | |
| [dnb-hugo-pwa](https://github.com/dnb-org/dnb-hugo-pwa) | Turn your site into a Progressive Web Application. Add caching, offline mode and favicon support. |
| [dnb-hugo-renderhooks](https://github.com/dnb-org/dnb-hugo-renderhooks) | render hooks for Markdown markup |
| [dnb-hugo-robots](https://github.com/dnb-org/dnb-hugo-robots) | Add a customizable robots.txt to your website. |
| [dnb-hugo-schema](https://github.com/dnb-org/dnb-hugo-schema) | |
| [dnb-hugo-search-algolia](https://github.com/dnb-org/dnb-hugo-search-algolia) | |
| [dnb-hugo-security](https://github.com/dnb-org/dnb-hugo-security) | Add a security.txt to your site with information about your preferred procedures to notify the developer team about security issues. |
| [dnb-hugo-sitemap](https://github.com/dnb-org/dnb-hugo-sitemap) | |
| [dnb-hugo-social](https://github.com/dnb-org/dnb-hugo-social) | |
| [dnb-hugo-workflows](https://github.com/dnb-org/dnb-hugo-workflows) | A collection of Github Actions/Workflows to optimise your work with GoHugo. |
| [dnb-hugo-youtube](https://github.com/dnb-org/dnb-hugo-youtube) | A shortcode and partial for lite and speedy youtube embeds. |

<!--lint disable no-missing-blank-lines -->
<!--- COMPONENTS END --->
