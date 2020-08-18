# Gorko

A tiny, Sass-powered utility class generator, with handy helpers, that helps you to power your front-ends with a single source of truth.

## Table of contents

- [Gorko](#gorko)
  * [Getting started](#getting-started)
  * [Configuration](#configuration)
    + [`$gorko-base-size` (optional)](#--gorko-base-size---optional-)
    + [`$gorko-size-scale` (optional)](#--gorko-size-scale---optional-)
    + [`$gorko-colors` (optional)](#--gorko-colors---optional-)
    + [`$gorko-config` (required)](#--gorko-config---required-)
    + [Breakpoints](#breakpoints)
  * [Utility Class Generator](#utility-class-generator)
      - [Example outputs](#example-outputs)
  * [Sass functions](#sass-functions)
    + [`get-color($key: string)`](#-get-color--key--string--)
      - [Example](#example)
    + [`get-config-value($key: string, $group: string)`](#get-config-value--key--string---group--string-)
      - [Example](#example-1)
    + [`get-size($ratio-key: string)`](#-get-size--ratio-key--string--)
      - [Example](#example-2)
  * [Sass mixins](#sass-mixins)
    + [`apply-utility($key: string, $value-key: string)`](#-apply-utility--key--string---value-key--string--)
      - [Example](#example-3)
    + [`media-query($key: string)`](#-media-query--key--string--)
      - [Example](#example-4)
  * [Contributing](#contributing)
  * [Pull Request Process](#pull-request-process)
  * [Code of Conduct](#code-of-conduct)
    + [Our Pledge](#our-pledge)
    + [Our Standards](#our-standards)
    + [Our Responsibilities](#our-responsibilities)
    + [Scope](#scope)
    + [Enforcement](#enforcement)
    + [Attribution](#attribution)

## Getting started

First up, install Gorko:

```bash
npm install gorko
```

In your Sass (SCSS in this case), import Gorko like so:

```scss
@import '../path/to/your/node_modules/gorko/gorko.scss';
```

This will generate utility classes based on the default configuration. To configure it for yourself, take this default, and create your own. Once it is created **import your config before Gorko**, like this:

```bash
@import 'config';
```

## Configuration

This is the default configuration. It is recommended that you use it as your base for your own configuration.

```scss
/// BASE SIZE
/// All calculations are based on this. It’s recommended that
/// you keep it at 1rem because that is the root font size. You
/// can set it to whatever you like and whatever unit you like.
///
$gorko-base-size: 1rem !default;

/// SIZE SCALE
/// This is a Major Third scale that powers all the utilities that
/// it is relevant for (font-size, margin, padding). All items are
/// calcuated off the base size, so change that and cascade across
/// your whole project.
///
$gorko-size-scale: (
  '300': $gorko-base-size * 0.8,
  '400': $gorko-base-size,
  '500': $gorko-base-size * 1.25,
  '600': $gorko-base-size * 1.6,
  '700': $gorko-base-size * 2,
  '900': $gorko-base-size * 3
) !default;

/// COLORS
/// Colors are shared between backgrounds and text by default.
/// You can also use them to power borders, fills or shadows, for example.
///
$gorko-colors: (
  'dark': #1a1a1a,
  'light': #f3f3f3
) !default;

/// CORE CONFIG
/// This powers everything from utility class generation to breakpoints
/// to enabling/disabling pre-built components/utilities.
///
$gorko-config: (
  'bg': (
    'items': $gorko-colors,
    'output': 'standard',
    'property': 'background'
  ),
  'color': (
    'items': $gorko-colors,
    'output': 'standard',
    'property': 'color'
  ),
  'box': (
    'items': (
      'block': 'block',
      'flex': 'flex',
      'hide': 'none',
      'show': 'inherit'
    ),
    'output': 'responsive',
    'property': 'display'
  ),
  'font': (
    'items': (
      'base': 'Helvetica, Arial, sans-serif'
    ),
    'output': 'standard',
    'property': 'font-family'
  ),
  'gap-top': (
    'items': $gorko-size-scale,
    'output': 'standard',
    'property': 'margin-top'
  ),
  'gap-right': (
    'items': $gorko-size-scale,
    'output': 'standard',
    'property': 'margin-right'
  ),
  'gap-bottom': (
    'items': $gorko-size-scale,
    'output': 'standard',
    'property': 'margin-bottom'
  ),
  'gap-left': (
    'items': $gorko-size-scale,
    'output': 'standard',
    'property': 'margin-left'
  ),
  'pad-top': (
    'items': $gorko-size-scale,
    'output': 'standard',
    'property': 'padding-top'
  ),
  'pad-right': (
    'items': $gorko-size-scale,
    'output': 'standard',
    'property': 'padding-right'
  ),
  'pad-bottom': (
    'items': $gorko-size-scale,
    'output': 'standard',
    'property': 'padding-bottom'
  ),
  'pad-left': (
    'items': $gorko-size-scale,
    'output': 'standard',
    'property': 'padding-left'
  ),
  'stack': (
    'items': (
      '300': 0,
      '400': 10,
      '500': 20,
      '600': 30,
      '700': 40
    ),
    'output': 'standard',
    'property': 'z-index'
  ),
  'text': (
    'items': $gorko-size-scale,
    'output': 'responsive',
    'property': 'font-size'
  ),
  'weight': (
    'items': (
      'light': '300',
      'regular': '400',
      'bold': '700'
    ),
    'output': 'standard',
    'property': 'font-weight'
  ),
  'width': (
    'items': (
      'full': '100%',
      'half': percentage(1/2),
      'quarter': percentage(1/4),
      'third': percentage(1/3)
    ),
    'output': 'responsive',
    'property': 'width'
  ),
  'breakpoints': (
    'sm': '(min-width: 36em)',
    'md': '(min-width: 48em)',
    'lg': '(min-width: 62em)'
  )
) !default;
```

### `$gorko-base-size` (optional)

The base size for the size ratio calculations. It is only required for the default configuration.

### `$gorko-size-scale` (optional)

This takes the base size and by default, generates a **major third** size scale. This can be set to whatever scale you like.

If this is not set, the `get-size` function will use the default configuration.

### `$gorko-colors` (optional)

A collection of key/value pairs that by default, generate text and background colour utilities.

If this is not set, the `get-color` function will use the default configuration.

### `$gorko-config` (required)

🚨 Without this set, Gorko won’t work. 🚨

It contains all of the utility class definitions and breakpoint definitions that the generator and mixins use.

You can add as many or as little utility class definitions as you like—likewise for breakpoint definitions.

### Breakpoints

The `breakpoints` map in `$gorko-config` defines media queries for the utility class generator. By default, the are set as follows:

```scss
'breakpoints': (
	'sm': '(min-width: 36em)',
	'md': '(min-width: 48em)',
	'lg': '(min-width: 62em)'
)
```

You can add as many or as little of these as you like and call them whatever you like. The only requirement is that the value is a valid media query.

## Utility Class Generator

The utility class generator loops through `$gorko-config` looking for items that have a valid utility class structure. The following structure is required to generate a utility class:

```scss
'width':('items':('full':'100%','half': '50%',
	),
	'output': 'standard',
	'property': 'width'
),;
```

The first key is the name of the utility and that contains a Sass map. Inside that map, you need to have the following: - `items`: a map of key/value pairs which link a utility class to a CSS property’s value - `output`: this must be `responsive` or `standard`. If you set it to `responsive`, it will generate the same utility class for **every breakpoint that is defined**. - `property`: the [CSS property](https://css-tricks.com/almanac/properties/) that this utility controls.

#### Example outputs

The above structure would output the following utility classes:

```css
.width-full {
  width: 100%;
}

.width-half {
  width: 50%;
}
```

If we set the `output` to be `responsive`, with the default `breakpoints` defined, the output would be as follows:

```css
.width-full {
  width: 100%;
}

.width-half {
  width: 50%;
}

@media screen and (min-width: 36em) {
  .sm\:width-full {
    width: 100%;
  }

  .sm\:width-half {
    width: 50%;
  }
}

@media screen and (min-width: 48em) {
  .md\:width-full {
    width: 100%;
  }

  .md\:width-half {
    width: 50%;
  }
}

@media screen and (min-width: 62em) {
  .lg\:width-full {
    width: 100%;
  }

  .lg\:width-half {
    width: 50%;
  }
}
```

## Sass functions

There are a couple of handy functions that give you access to configuration settings.

### `get-color($key: string)`

Takes the passed `$key` and attempts to retrieve a match from `$gorko-colors`.

#### Example

Using the default config:

```scss
$dark = get-color('dark'); // #1a1a1a
```

### `get-config-value($key: string, $group: string)`

Returns back a 1 dimensional (key value pair) config value if available.

#### Example

Using the default config:

```scss
$light-weight: get-config-value('light', 'weight'); // 300
```

### `get-size($ratio-key: string)`

Tries to match the passed `$ratio-key` with the `$gorko-size-scale`. Returns null if it can’t find a match.

#### Example

Using the default config:

```scss
$my-size: get-size('500'); // 1.25rem
```

## Sass mixins

### `apply-utility($key: string, $value-key: string)`

Grab the property and value of one of the `$gorko-config` utilities that the generator will generate a class for.

#### Example

Using the default config:

```scss
.my-element {
  @include apply-utility('weight', 'bold'); // font-weight: bold;
}
```

### `media-query($key: string)`

Pass in the key of one of your breakpoints set in `$gorko-config['breakpoints']` and this mixin will generate the media query with your configured value.

#### Example

Using the default config:

```scss
.my-element {
  @include media-query('md') {
    background: goldenrod;
  }
}
```

Output:

```css
@media (min-width: 48em) {
  .my-element {
    background: goldenrod;
  }
}
```

## Contributing

When contributing to this repository, please first discuss the change you wish to make via issue,
email, or any other method with the owners of this repository before making a change.

Please note we have a code of conduct, please follow it in all your interactions with the project.

## Pull Request Process

1. Ensure any install or build dependencies are removed before the end of the layer when doing a
   build.
2. Ensure your work is thoroughly tested, to the best of your abilities
3. You may merge the Pull Request in once you have the sign-off from a maintainer

## Code of Conduct

### Our Pledge

In the interest of fostering an open and welcoming environment, we as
contributors and maintainers pledge to making participation in our project and
our community a harassment-free experience for everyone, regardless of age, body
size, disability, ethnicity, gender identity and expression, level of experience,
nationality, personal appearance, race, religion, or sexual identity and
orientation.

### Our Standards

Examples of behavior that contributes to creating a positive environment
include:

- Using welcoming and inclusive language
- Being respectful of differing viewpoints and experiences
- Gracefully accepting constructive criticism
- Focusing on what is best for the community
- Showing empathy towards other community members

Examples of unacceptable behavior by participants include:

- The use of sexualized language or imagery and unwelcome sexual attention or
  advances
- Trolling, insulting/derogatory comments, and personal or political attacks
- Public or private harassment
- Publishing others' private information, such as a physical or electronic
  address, without explicit permission
- Other conduct which could reasonably be considered inappropriate in a
  professional setting

### Our Responsibilities

Project maintainers are responsible for clarifying the standards of acceptable
behavior and are expected to take appropriate and fair corrective action in
response to any instances of unacceptable behavior.

Project maintainers have the right and responsibility to remove, edit, or
reject comments, commits, code, wiki edits, issues, and other contributions
that are not aligned to this Code of Conduct, or to ban temporarily or
permanently any contributor for other behaviors that they deem inappropriate,
threatening, offensive, or harmful.

### Scope

This Code of Conduct applies both within project spaces and in public spaces
when an individual is representing the project or its community. Examples of
representing a project or community include using an official project e-mail
address, posting via an official social media account, or acting as an appointed
representative at an online or offline event. Representation of a project may be
further defined and clarified by project maintainers.

### Enforcement

Instances of abusive, harassing, or otherwise unacceptable behavior may be
reported by contacting the project team at me@andy-bell.design. All
complaints will be reviewed and investigated and will result in a response that
is deemed necessary and appropriate to the circumstances. The project team is
obligated to maintain confidentiality with regard to the reporter of an incident.
Further details of specific enforcement policies may be posted separately.

Project maintainers who do not follow or enforce the Code of Conduct in good
faith may face temporary or permanent repercussions as determined by other
members of the project's leadership.

### Attribution

This Code of Conduct is adapted from the [Contributor Covenant][homepage], version 1.4,
available at [http://contributor-covenant.org/version/1/4][version]

[homepage]: http://contributor-covenant.org
[version]: http://contributor-covenant.org/version/1/4/
