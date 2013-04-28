# SUIT grid layouts (responsive)

A SUIT component that extends [suit-grid](https://github.com/necolas/suit-grid/)
to provide responsive CSS layouts across 2 Media Query breakpoints.

It aims to make it easier to build larger, intricate, adaptive layouts from
simple HTML/CSS building blocks.

Read more about [SUIT's design principles](https://github.com/necolas/suit/).

## Installation

* [Bower](http://bower.io/): `bower install --save suit-grid-layouts`
* Download: [zip](https://github.com/necolas/suit-grid-layouts/zipball/master)
* Git: `git clone https://github.com/necolas/suit-grid-layouts`

## Features

This component takes a slightly different approach to CSS grids. In addition to
the features inherited from [suit-grid](https://github.com/necolas/suit-grid/),
this responsive extension includes:

* Narrow/mobile-first.
* Fluid across all breakpoints.
* Control over individual parts of layout across multiple viewport widths.
* Support for 2 breakpoints at which layout transformations can occur.
* Easy to extend with further layout changes for even wider viewports.
* Infinite nesting of adaptive layouts.
* Complex custom layouts.

This grid extension is a collection of simple, prebuilt, self-contained fluid
layouts. In contrast, flexible responsive grid systems like
[griddle](https://github.com/necolas/griddle/) use classes to directly control
the percentage width of _individual_ elements at multiple viewport widths.

## Layouts

Layouts are inherited by wider viewports.

### v1: narrow-width viewports (default)

* `v1-Grid--2col`: Split into 2 columns
* `v1-Grid--3col`: Split into 3 columns

### v2: medium-width viewports (>= 25em)

* `v2-Grid--1col`: One column
* `v2-Grid--2col`: Split into 2 columns
* `v2-Grid--3col`: Split into 3 columns
* `v2-Grid--4col`: Split into 4 columns
* `v2-Grid--1to2`: Split into 2 columns, 1:2 ratio (requires 2 cells)
* `v2-Grid--2to1`: Split into 2 columns, 2:1 ratio (requires 2 cells)
* `v2-Grid--1to3`: Split into 2 columns, 1:3 ratio (requires 2 cells)
* `v2-Grid--3to1`: Split into 2 columns, 3:1 ratio (requires 2 cells)
* `v2-Grid--3on1`: Split into 3 columns stacked on 1 column (requires 4 cells)
* `v2-Grid--fitToFill`: Split into 2 columns, fit:fill ratio (requires 2 cells)

### v3: wide-width viewports (>= 50em)

* `v2-Grid--1col`: One column
* `v3-Grid--2col`: Split into 2 columns
* `v3-Grid--3col`: Split into 3 columns
* `v3-Grid--4col`: Split into 4 columns
* `v3-Grid--1to2`: Split into 2 columns, 1:2 ratio (requires 2 cells)
* `v3-Grid--2to1`: Split into 2 columns, 2:1 ratio (requires 2 cells)
* `v3-Grid--2to3`: Split into 2 columns, 2:3 ratio (requires 2 cells)
* `v3-Grid--3to2`: Split into 2 columns, 3:2 ratio (requires 2 cells)

## Use

### Referencing the components

During development, you can include the grid components in your CSS using the
`@import` directive in your main stylesheet. Include your custom Media Query
breakpoints here too. Your build step should take care of inlining these
imports for IE 8 testing and production.

Example:

```css
@import "/bower_components/suit-grid/grid.css";
@import "/bower_components/suit-grid-layouts/grid-layouts-v1.css";
@import "/bower_components/suit-grid-layouts/grid-layouts-v2.css" (min-width: 25em);
@import "/bower_components/suit-grid-layouts/grid-layouts-v3.css" (min-width: 50em);
```

### Templating

A basic SUIT grid looks like this:

```html
<div class="Grid">
    <div class="Grid-cell">…</div>
    <div class="Grid-cell">…</div>
</div>
```

It provides the core layout mechanism for grid layouts. From this base,
different cells can be given different widths (e.g., by using [SUIT's dimension
utilities](https://github.com/necolas/suit-utils-dimension)).

The SUIT Grid Layouts component relies on adding modifier classes to the `Grid`
and `Grid-cell` elements.

* `v[n]-Grid--*`: A layout type (`*`) matched to a minimum breakpoint (`n`).
* `Grid-cell--[n]`: A cell that has source order position `n`.

In the following example, the addition of `v1-Grid--2col` to the main grid
results in 2 equal-width columns at all viewport sizes (due to layout
inheritance).

```html
<div class="Grid v1-Grid--2col">
    <div class="Grid-cell Grid-cell--1">…</div>
    <div class="Grid-cell Grid-cell--2">…</div>
    <div class="Grid-cell Grid-cell--3">…</div>
    <div class="Grid-cell Grid-cell--4">…</div>
</div>
```

To make this grid change to 4 equal-width columns at the next breakpoint, add
the `v2-Grid--4col` class:

```html
<div class="Grid v1-Grid--2col v2-Grid--4col">
    <div class="Grid-cell Grid-cell--1">…</div>
    <div class="Grid-cell Grid-cell--2">…</div>
    <div class="Grid-cell Grid-cell--3">…</div>
    <div class="Grid-cell Grid-cell--4">…</div>
</div>
```

The following example is a grid that changes from being a single column of
cells by default (at narrow viewports - v1), to two columns (at medium
viewports - v2), to 4 columns (at wide viewports - v3):

```html
<div class="Grid v2-Grid--2col v3-Grid--4col">
    <div class="Grid-cell Grid-cell--1">…</div>
    <div class="Grid-cell Grid-cell--2">…</div>
    <div class="Grid-cell Grid-cell--3">…</div>
    <div class="Grid-cell Grid-cell--4">…</div>
</div>
```

Nesting grids is easy. Each grid adapts independently of its context. In
the follow example, a grid changes from 1 column (narrow) to 2 columns (medium,
and wide). A second grid is nested within the first cell of this grid.
This nested grid changes from 2 columns (narrow, and medium) to 4 columns
(wide) using the same global breakpoints.

```html
<!-- outer grid -->
<div class="Grid v2-Grid--2col">
    <div class="Grid-cell Grid-cell--1">
        <!-- inner grid -->
        <div class="Grid v1-Grid--2col v3-Grid--4col">
            <div class="Grid-cell Grid-cell--1">…</div>
            <div class="Grid-cell Grid-cell--2">…</div>
            <div class="Grid-cell Grid-cell--3">…</div>
            <div class="Grid-cell Grid-cell--4">…</div>
        </div>
    </div>
    <div class="Grid-cell Grid-cell--2">
        …
    </div>
</div>
```

#### Template inheritance

If you use a templating system that provides template inheritance, then this
can be a particularly effective way to abstract layout. The following example
uses [Hogan](https://github.com/twitter/hogan.js)'s template inheritance
functionality.

Reusable layout template:

```html
<!-- @name grid_doubles
     @desc A stand-alone template for the 1col -> 2col -> 4col layout flow -->

<div class="Grid v2-Grid--2col v3-Grid--4col">
    <div class="Grid-cell Grid-cell--1">
        {{$content_cell_1}}{{/content_cell_1}}>
    </div>
    <div class="Grid-cell Grid-cell--2">
        {{$content_cell_2}}{{/content_cell_2}}>
    </div>
    <div class="Grid-cell Grid-cell--3">
        {{$content_cell_3}}{{/content_cell_3}}>
    </div>
    <div class="Grid-cell Grid-cell--4">
        {{$content_cell_4}}{{/content_cell_4}}>
    </div>
</div>
```

Component-specific template that inherits a reusable layout template:

```html
<!-- @name promos
     @desc Layout for homepage promo boxes -->

{{< grid_doubles}}
    {{$content_cell_1}}
        {{> partials/promo_1}}
    {{/content_cell_1}}
    {{$content_cell_2}}
        {{> partials/promo_2}}
    {{/content_cell_2}}
    {{$content_cell_3}}
        {{> partials/promo_3}}
    {{/content_cell_3}}
    {{$content_cell_4}}
        {{> partials/promo_4}}
    {{/content_cell_4}}
{{/grid_doubles}}
```

### IE 8 support

If you need to support a widescreen layout for IE 8 (without resorting to
JavaScript), you may want to look into serving the grid CSS to IE 8
[without using media
queries](http://nicolasgallagher.com/mobile-first-css-sass-and-ie/)).

## Contributing to development

Front-end development dependencies are managed with
[Bower](http://bower.io/), a node package.

Install Bower:

```
npm install -g bower
```

From within the `suit-grid-layouts` directory, run:

```
bower install
```

This will install [normalize.css](https://github.com/necolas/normalize.css/)
and [suit-grid](https://github.com/necolas/suit-grid/) in the `components`
directory. These components are used by the HTML test files found in `test`.

When submitting patches, you should include a corresponding test in the HTML
files and check that you have not introduced any regressions.

Please read these [contribution
guidelines](https://github.com/necolas/issue-guidelines/blob/master/CONTRIBUTING.md).

## Browser support

* Google Chrome (latest)
* Opera (latest)
* Firefox 4+
* Safari 5+
* Internet Explorer 9+ (IE 8 requires a build step)
