# native-font
**Native font stacks for web wizards**

## About

This library intends to help you make use of native font stacks on the web.

It comes bundled with 76 pre-defined fonts. But don't worry! You only ship to
the browser what you need. And these are native fonts, so there won't be any
downloading of font files. It's just the definitions needed to use the fonts
that are pre-installed on the end user machine.


## Install

```console
npm install --save-dev native-font
```

## Include

### mixins

```scss
@use "native-font";

.some-rule {
  @include native-font.variables();
}
```

### fonts

```scss
@use "native-font/arial";
@use "native-font/arial-black";
@use "native-font/courier-new";
```

## Use

Lets first look at how to [apply a font](#apply-a-font) to your text and cover
how to [define a set of fonts](#define-a-set-of-fonts) and
[output the styles](#output-the-styles) later.

### Apply a font

You apply a font by assigning a CSS variable to the `font-family`
property of the element(s) you want to style. For example:

```html
<p>Text in my font</p>
```

```css
p {
  font-family: var(--arial);
}
```

This would assign the font `arial` to the text in the `p` element. The CSS
variable name `--arial` can be easily derived from the name of the font. For
example, for Arial Black it would be `--arial-black` and for Segoe UI it would
be `--segoe-ui`.

Now lets look at how to define a set of fonts and output the styles for it.

### Define a set of fonts

To define a set of fonts, you can [use sass](#use-sass) (recommended), or you
can [use a predefined set](#use-a-predefined-set),

#### Use sass
With sass you can automatically build up the font set as you `@use` fonts in
your code:

```scss
// load the 'arial' font
@use "native-font/arial";
// load more fonts if you need
// a dependency tree is created automatically
```

Dependencies are automatically resolved because each font actually `@use`s the
fonts it depends on and whenever a font is used for the first time, it is added
to the set.

> Font stacks list the most specific fonts first, then less specific fonts as
> fallbacks for when the most specific font is not available. For example, the
> font stack for Arial might look something like `Arial, Helvetica, sans-serif`,
> which means so much as 'use Arial, but if that is not available, use Helvetica.
> If that too is not available, use any sans-serif font'. So it makes sense to
> think of fonts as 'deriving' from generic fonts. And that is exactly what this
> library does. That is also why the predefined sets start with the most generic
> fonts like `serif`, `sans-serif` and `monospace` first and we only add the more
> specific fonts later. Because those fonts actually build on top of the more
> generic fonts in the stack.

#### Use a predefined set
There are also some predefined sets available that you can load:

* [`w3c`](#w3c): The family names defined by the World Wide Web Consortium
* [`base`](#base): The w3C fonts plus 3 extra generic names
* [`modern](#modern): A set of modern web fonts
* [`cssfonts](#cssfonts): The web-safe fonts we know and love
* [`all`](#all): The complete set of all of the above

You can also load these predefined sets by importing the CSS stylesheet:

```css
@import 'native-font/modern.css';
```

### Output the styles

Once we have defined the set of fonts we want to use, we need to output the
styles for them. That is actually as simple as this:

```scss
// load native-font mixins
@use "native-font";
// load fonts
@use "native-font/arial";
// output the styles
:root, ::backdrop {
  // render the CSS variables
  @include native-font.variables();
}
```

Outputs:

```css
:root, ::backdrop {
  --color-emoji: "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
  --sans-serif: "Helvetica Neue", "Arial Nova", "Nimbus Sans", Helvetica, Arial, sans-serif, var(--color-emoji);
  --arial: "Arial Nova", Arial, "Helvetica Neue", Helvetica, var(--sans-serif);
}
```

As you can see, you actually get 3 variables. This font stack defines an extra
generic font name for `color-emoji` and makes it the fallback for the other
generic fonts, in order to improve emoji support across devices. The generic
fonts themselves are also defined as font stacks, which is why we get
`sans-serif` defined here as well. Because that is the fallback for `arial`,
the font we actually selected.


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

