# native-font
Native font stacks for web wizards

## Native font
Native fonts are fonts that are expected to be present on the user device.
Thus, they need not be downloaded, saving precious time. Because not all
devices support all fonts, the web defines standards for presenting a list
of fonts to be used as fallbacks if the requested font does not exist. The
resulting list of (fallback) fonts is called a font-stack.

This library intends to help you make use of native font stacks on the web.
It comes bundled with 76 pre-defined font-stacks. But don't worry! You only
ship to the browser what you need. And these are native fonts, so there
won't be any downloading of font files.

## Install
```console
npm install --save-dev native-font
```

## Use

This library composes font stacks by creating CSS variables, which can in turn
contain other CSS-variables. This way we can create font stacks by reusing
other font stacks without having to repeat ourselves.

Here is how that works:

```css
/* native-font stack */
:root, ::backdrop {
  --color-emoji: "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
  --sans-serif: "Helvetica Neue", "Arial Nova", "Nimbus Sans", Helvetica, Arial, sans-serif, var(--color-emoji);
  --arial: "Arial Nova", Arial, "Helvetica Neue", Helvetica, var(--sans-serif);
}

.my-font {
  /* use native-font */
  font-family: var(--arial);
}
```

What this actually does, is set the `font-family` to the contents of the
CSS variable `--arial`, which in turn references `--sans-serif` and
`--color-emoji` to end up effectively assigning this font stack:

```css
.my-font {
  /* effective result */
  font-family: "Arial Nova", Arial, "Helvetica Neue", Helvetica,
      "Helvetica Neue", "Arial Nova", "Nimbus Sans", Helvetica,
      Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji",
      "Segoe UI Symbol", "Noto Color Emoji";
}
```css

This does contain duplicates, but those are harmless. The first occurrance
that matches will always win. And this does actually give us a very compact
yet powerful way to express font stacks.

Ok, so how to include the CSS variables in the CSS? Either you do it yourself
with Sass (recommended), or you use one of the pre-baked sets `w3c.css`,
`base.css`, `modern.css`, `cssfonts.css` or `all.css`.

### Use with Sass

In your sass files, `@use "native-font/arial"` (for example) and then use the provided `variabes` mixin to generate the CSS variables needed to use this font.

Example:

*app.scss*

```scss
// load native-font
@use "native-font";
// load the 'arial' font
@use "native-font/arial";
// use the mixin
:root, ::backdrop {
  @include native-font.variables();
}
```

Produces:

*app.css*
```css
:root, ::backdrop {
  --color-emoji: "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
  --sans-serif: "Helvetica Neue", "Arial Nova", "Nimbus Sans", Helvetica, Arial, sans-serif, var(--color-emoji);
  --arial: "Arial Nova", Arial, "Helvetica Neue", Helvetica, var(--sans-serif);
}
```

As you can see, you actually get 3 variables. This font stack defines an extra
generic font name for `color-emoji' and makes it the fallback for the other
generic fonts, in order to improve emoji support across devices. The generic
fonts themselves are also defined as font stacks, which is why we get
`sans-serif` defined here as well. Because that is the fallback for `arial`,
the font we actually selected.

Dependencies are automatically resolved because each font actually `@use`s the
fonts it depends on and whenever a font is used for the first time, it is added
to the collection of fonts in use.

## Available fonts (by set)

### [w3c](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family)

```scss
@use "native-font/sets/w3c";
```

Contains **14 fonts** (± 1kB):

* `color-emoji` (core)
* [`cursive`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#cursive) (generic)
* [`emoji`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#emoji) (generic)
* [`fangsong`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#fangsong) (generic)
* [`fantasy`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#fantasy) (generic)
* [`math`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#math) (generic)
* [`monospace`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#monospace) (generic)
* [`sans-serif`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#sans-serif) (generic)
* [`serif`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#serif) (generic)
* [`system-ui`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#system-ui) (ui)
* [`ui-sans-serif`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#ui-sans-serif) (ui)
* [`ui-serif`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#ui-serif) (ui)
* [`ui-monospace`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#ui-monospace) (ui)
* [`ui-rounded`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#ui-rounded) (ui)

### base

```scss
@use "native-font/sets/base";
```

Contains **17 fonts** (± 2kB), all 14 from [`w3c`](#w3c), plus these 3:

* [`emojicon`](https://npmjs.com/package/magicon) (generic-extra)
* `ui-emoji` (generic-extra)
* `ui-emojicon` (generic-extra)

### [modern](https://modernfontstacks.com)

*scss*

```scss
@use "native-font/sets/modern";
```

Contains **28 fonts** (± 3kB), all 17 from [`base`](#base), plus these 11:

* [`antique`](https://modernfontstacks.com/#antique)
* [`classical-humanist`](https://modernfontstacks.com/#classical-humanist)
* [`didone`](https://modernfontstacks.com/#didone)
* [`geometric-humanist`](https://modernfontstacks.com/#geometric-humanist)
* [`handwritten`](https://modernfontstacks.com/#handwritten)
* [`industrial`](https://modernfontstacks.com/#industrial)
* [`monospace-code`](https://modernfontstacks.com/#monospace-code)
* [`monospace-slab-serif`](https://modernfontstacks.com/#monospace-slab-serif)
* [`rounded-sans`](https://modernfontstacks.com/#rounded-sans)
* [`slab-serif`](https://modernfontstacks.com/#slab-serif)
* [`transitional`](https://modernfontstacks.com/#transitional)


### [cssfonts](https://www.cssfontstack.com/)

*scss*

```scss
@use "native-font/sets/cssfonts";
```

Contains **64 fonts** (± 5kB), all 17 from [`base`](#base), plus these 47:

* [`andale-mono`](https://www.cssfontstack.com/Andale-Mono)
* [`arial`](https://www.cssfontstack.com/Arial)
* [`arial-black`](https://www.cssfontstack.com/Arial-Black)
* [`arial-narrow`](https://www.cssfontstack.com/Arial-Narrow)
* [`arial-rounded-mt-bold`](https://www.cssfontstack.com/Arial-Rounded-MT-Bold)
* [`avant-garde`](https://www.cssfontstack.com/Avant-Garde)
* [`baskerville-old-face`](https://www.cssfontstack.com/Baskerville-Old-Face)
* [`baskerville`](https://www.cssfontstack.com/Baskerville)
* [`big-caslon`](https://www.cssfontstack.com/Big-Caslon)
* [`bodoni-mt`](https://www.cssfontstack.com/Bodoni-MT)
* [`book-antiqua`](https://www.cssfontstack.com/Book-Antiqua)
* [`calibri`](https://www.cssfontstack.com/calibri)
* [`calisto-mt`](https://www.cssfontstack.com/calisto-mt)
* [`cambria`](https://www.cssfontstack.com/Cambria)
* [`candara`](https://www.cssfontstack.com/Candara)
* [`century-gothic`](https://www.cssfontstack.com/Century-Gothic)
* [`consolas`](https://www.cssfontstack.com/Consolas)
* [`copperplate`](https://www.cssfontstack.com/Copperplate)
* [`courier-new`](https://www.cssfontstack.com/Courier-New)
* [`didot`](https://www.cssfontstack.com/Didot)
* [`franklin-gothic-medium`](https://www.cssfontstack.com/Franklin-Gothic-Medium)
* [`futura`](https://www.cssfontstack.com/Futura)
* [`garamond`](https://www.cssfontstack.com/Garamond)
* [`geneva`](https://www.cssfontstack.com/Geneva)
* [`georgia`](https://www.cssfontstack.com/Georgia)
* [`gill-sans`](https://www.cssfontstack.com/Gill-Sans)
* [`goudy-old-style`](https://www.cssfontstack.com/Goudy-Old-Style)
* [`helvetica`](https://www.cssfontstack.com/Helvetica)
* [`hoefler-text`](https://www.cssfontstack.com/Hoefler-Text)
* [`impact`](https://www.cssfontstack.com/Impact)
* [`lucida-bright`](https://www.cssfontstack.com/Lucida-Bright)
* [`lucida-console`](https://www.cssfontstack.com/Lucida-Console)
* [`lucida-grande`](https://www.cssfontstack.com/Lucida-Grande)
* [`lucida-sans-typewriter`](https://www.cssfontstack.com/Lucida-Sans-Typewriter)
* [`monaco`](https://www.cssfontstack.com/Monaco)
* [`optima`](https://www.cssfontstack.com/Optima)
* [`palatino`](https://www.cssfontstack.com/Palatino)
* [`papyrus`](https://www.cssfontstack.com/Papyrus)
* [`perpetua`](https://www.cssfontstack.com/Perpetua)
* [`rockwell`](https://www.cssfontstack.com/Rockwell)
* [`rockwell-extra-bold`](https://www.cssfontstack.com/Rockwell-Extra-Bold)
* [`segoe-ui`](https://www.cssfontstack.com/segoe-ui)
* [`tahoma`](https://www.cssfontstack.com/Tahoma)
* [`times`](https://www.cssfontstack.com/Times)
* [`times-new-roman`](https://www.cssfontstack.com/Times-New-Roman)
* [`trebuchet-ms`](https://www.cssfontstack.com/Trebuchet-MS)
* [`verdana`](https://www.cssfontstack.com/Verdana)


### all

*scss*

```scss
@use "native-font/sets/all";
```

Contains **76 fonts**: (± 6kB)

* [`w3c`](#w3c): 14 fonts (± 1kB)
* [`base`](#base): 3 + 14 makes 17 fonts (± 2kB)
* [`modern`](#modern): 11 + 17 makes 28 fonts (± 3kB)
* [`cssfonts`](#cssfonts): 47 + 17 makes 64 fonts (± 5kB)
