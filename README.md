# SUIT responsive grid

A SUIT component that extends [suit-grid](https://github.com/necolas/suit-grid)
to provide different layouts at different media query breakpoints.

It includes [narrow-first](grid-responsive.css) and
[wide-first](grid-responsive-alt.css) style sheets for use with different
approaches to development. You should only use one of these at a time.

Read more about [SUIT's design principles](https://github.com/necolas/suit/).

## Installation

* Download: [zip](https://github.com/necolas/suit-grid/zipball/master)
* [Bower](https://github.com/twitter/bower/): `bower install suit-grid-responsive --save`
* Git: `git clone https://github.com/necolas/suit-grid`

## Features

This responsive grid component makes it possible to build intricate, adaptive
layouts from simple principles. It provides a toolkit to build layouts that
flow between 3 distinct breakpoints. The implementation makes it possible to
think about how individual parts of the page adapt at different breakpoints. In
addition to the features inherited from
[suit-grid](https://github.com/necolas/suit-grid), this responsive extension
features:

* Narrow/mobile-first.
* Support for 3 distinct verions of global layout at narrow, medium, and wide
  viewports (using 2 breakpoints).
* Easy to extend with further layout changes for even wider viewports.
* Infinite nesting of adaptive layouts.
* Complex custom layouts.

## Use

If you need to support a widescreen layout for IE 8 (without resorting to
JavaScript) you may want to look into a way of serving the grid CSS to IE 8
[without including media
queries](http://nicolasgallagher.com/mobile-first-css-sass-and-ie/)).

### Referencing the components

During development, you can include the grid components in your CSS using the
`@import` directive in your main stylesheet. Your build step should take care
of inlining these imports for production.

Example:

```css
@import "/components/suit-grid/grid.css";
@import "/components/suit-grid-responsive/grid-responsive.css";
```

### Templating

In an HTML template, include the `Grid` modifier class that maps to the layout
pattern you want for a particular breakpoint. Make sure each `Grid-cell` has a
modifier class that denotes its place in the grid, i.e., `Grid-cell--1`,
`Grid-cell--2`, etc.

The following, example would be a grid that changes from being a single column
of cells by default (at narrow viewports - v1), to two columns (at medium
viewports - v2), to 4 columns (at wide viewports - v3):

```html
<div class="Grid Grid--v2-2col Grid--v3-4col">
    <div class="Grid-cell Grid-cell--1">
        […]
    </div>
    <div class="Grid-cell Grid-cell--2">
        […]
    </div>
    <div class="Grid-cell Grid-cell--3">
        […]
    </div>
    <div class="Grid-cell Grid-cell--4">
        […]
    </div>
</div>
```

Nesting grids is easy to do. Each grid adapts independently of its context. In
the follow example, a grid changes from 1 column (narrow) to 2 columns (medium,
and wide). A second grid is nested within the first child cell of this grid.
This second grid changes from 2 columns (narrow, and medium) to 4 columns
(wide) using the same global breakpoints.

```html
<!-- outer grid -->
<div class="Grid Grid--v2-2col">
    <div class="Grid-cell Grid-cell--1">
        <!-- inner grid -->
        <div class="Grid Grid--v1-2col Grid-v3-4col">
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

<div class="Grid Grid--v2-2col Grid--v3-4col">
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

## Browser support

* Google Chrome (latest)
* Opera (latest)
* Firefox 4+
* Safari 5+
* Internet Explorer 8+
