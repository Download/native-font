# native-font
Native font stacks for web wizards

## Native font
Native fonts are fonts that are expected to be present on the user device.
Thus, they need not be downloaded, saving precious time. Because not all
devices support all fonts, the web defines standards for presenting a list
of fonts to be used as fallbacks if the requested font does not exist. The
resulting list of (fallback) fonts is called a font-stack.

This library intends to help you make use of native font stacks on the web.

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
  --sans-serif: Helvetica, Arial, sans-serif, var(--color-emoji);
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
      Helvetica, Arial, sans-serif, "Apple Color Emoji",
      "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
}
```css

This does contain duplicates, but those are harmless. The first occurrance
will always win. And this does actually give us a very compact yet powerful
way to express font stacks.

Ok, so how to include the CSS variables in the CSS? Either you do it yourself
with Sass (recommended), or you use one of the pre-baked sets `base.css`,
`modern.css`, `cssfonts.css` or `all.css`.


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
  --sans-serif: Helvetica, Arial, sans-serif, var(--color-emoji);
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

### base (22 fonts)

*scss*

```scss
@use "native-font/sets/base";
```

Contains **22 fonts**:

* `color-emoji` (core)
* [`cursive`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#cursive) (w3c)
* [`emoji`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#emoji) (w3c)
* [`fangsong`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#fangsong) (w3c)
* [`fantasy`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#fantasy) (w3c)
* [`math`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#math) (w3c)
* [`monospace`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#monospace) (w3c)
* [`sans-serif`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#sans-serif) (w3c)
* [`serif`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#serif) (w3c)
* [`system-ui`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#system-ui) (w3c)
* `sans-serif-ui` (extra)
* [`ui-sans-serif`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#ui-sans-serif) (w3c)
* [`ui-serif`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#ui-serif) (w3c)
* `monospace-console` (extra)
* `monospace-code` (extra)
* [`ui-monospace`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#ui-monospace) (w3c)
* `rounded-sans` (extra)
* [`ui-rounded`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#ui-rounded) (w3c)
* `sans-serif-condensed` (extra)
* `sans-serif-nova` (extra)
* [`magicon`](https://npmjs.com/package/magicon) (extra)

### [modern](https://modernfontstacks.com)

*scss*

```scss
@use "native-font/sets/modern";
```

Contains **31 fonts**, all from [`base`](#base), plus:

* [`antique`](https://modernfontstacks.com/#antique)
* [`classical-humanist`](https://modernfontstacks.com/#classical-humanist)
* [`didone`](https://modernfontstacks.com/#didone)
* [`geometric-humanist`](https://modernfontstacks.com/#geometric-humanist)
* [`handwritten`](https://modernfontstacks.com/#handwritten)
* [`industrial`](https://modernfontstacks.com/#industrial)
* [`monospace-slab-serif`](https://modernfontstacks.com/#monospace-slab-serif)
* [`slab-serif`](https://modernfontstacks.com/#slab-serif)
* [`transitional`](https://modernfontstacks.com/#transitional)


### [cssfonts](https://www.cssfontstack.com/)

*scss*

```scss
@use "native-font/sets/cssfonts";
```

Contains **57 fonts**, all from [`base`](#base), plus:

* [`arial`](https://www.cssfontstack.com/Arial)
* [`arial-black`](https://www.cssfontstack.com/Arial-Black)
* [`arial-narrow`](https://www.cssfontstack.com/Arial-Narrow)
* [`arial-rounded-mt-bold`](https://www.cssfontstack.com/Arial-Rounded-MT-Bold)
* [`avant-garde`](https://www.cssfontstack.com/Avant-Garde)
* [`times`](https://www.cssfontstack.com/Times)
* [`times-new-roman`](https://www.cssfontstack.com/Times-New-Roman)
* [`garamond`](https://www.cssfontstack.com/Garamond)
* [`baskerville-old-face`](https://www.cssfontstack.com/Baskerville-Old-Face)
* [`baskerville`](https://www.cssfontstack.com/Baskerville)
* [`big-caslon`](https://www.cssfontstack.com/Big-Caslon)
* [`bodoni-mt`](https://www.cssfontstack.com/Bodoni-MT)
* [`palatino`](https://www.cssfontstack.com/Palatino)
* [`book-antiqua`](https://www.cssfontstack.com/Book-Antiqua)
* [`calibri`](https://www.cssfontstack.com/calibri)
* [`calisto-mt`](https://www.cssfontstack.com/calisto-mt)
* [`cambria`](https://www.cssfontstack.com/Cambria)
* [`candara`](https://www.cssfontstack.com/Candara)
* [`century-gothic`](https://www.cssfontstack.com/Century-Gothic)
* [`didot`](https://www.cssfontstack.com/Didot)
* [`franklin-gothic-medium`](https://www.cssfontstack.com/Franklin-Gothic-Medium)
* [`futura`](https://www.cssfontstack.com/Futura)
* [`geneva`](https://www.cssfontstack.com/Geneva)
* [`georgia`](https://www.cssfontstack.com/Georgia)
* [`gill-sans`](https://www.cssfontstack.com/Gill-Sans)
* [`goudy-old-style`](https://www.cssfontstack.com/Goudy-Old-Style)
* [`helvetica`](https://www.cssfontstack.com/Helvetica)
* [`hoefler-text`](https://www.cssfontstack.com/Hoefler-Text)
* [`impact`](https://www.cssfontstack.com/Impact)
* [`verdana`](https://www.cssfontstack.com/Verdana)
* [`lucida-grande`](https://www.cssfontstack.com/Lucida-Grande)
* [`optima`](https://www.cssfontstack.com/Optima)
* [`segoe-ui`](https://www.cssfontstack.com/segoe-ui)
* [`tahoma`](https://www.cssfontstack.com/Tahoma)
* [`trebuchet-ms`](https://www.cssfontstack.com/Trebuchet-MS)


### all

*scss*

```scss
@use "native-font/sets/all";
```

Contains **66 fonts**:

* [`base`](#base): 22 fonts
* [`modern`](#modern): 9 fonts (+ 22 from base)
* [`cssfonts`](#cssfonts): 35 fonts (+ 22 from base)
