# gr.scss

A small and customizable flexbox based grid system, written in SCSS.

By default gr.scss generates just the classes you need to get started working on your grid based layouts.
You can enable additional features as soon as you need them. By that, you  keep your CSS as small as possible.

- [Installation](#installation)
- [Setup](#setup)
  - [Settings](#settings)
    - [Grid without breakpoints](#grid-without-breakpoints)
    - [Grid with breakpoint definitions](#grid-with-breakpoint-definitions)
  - [Generate Classes](#generate-Classes)
- [Classes](#classes)
  - [Base](#base)
    - [Container](#container)
    - [Row](#row)
    - [Columns](#columns)
  - [Features](#features)
    - [row-full](#row-full)
    - [row-column](#row-column)
    - [row-reverse](#row-reverse)
    - [row-column-reverse](#row-column-reverse)
    - [prefix](#prefix)
    - [suffix](#suffix)
    - [push](#push)
    - [pull](#pull)
    - [align](#align)
    - [hide-show](#hide-show)
    - [no-gutter](#no-gutter)
    - [order](#order)
- [Helpers](#helpers)
  - [gr-media](#gr-media)
  - [gr-get-value](#gr-get-value)
- [Browser Support](#browser-Support)

## Installation

```
npm install gr.scss
```

## Setup

Simply import the grid, apply your configuration and let gr.scss generate the classes for you:

```scss
// Import gr.scss
@import 'node_modules/gr.scss/index';

// Your custom settings
$gr-columns: 12;
$gr-gutter: 24px;
[...]

// Generate classes
@include gr-build-classes();
```

### Settings

You can customize the class names of your grid and control the rendering of grid element styles that aren't required (e.g. styles for containers) by providing the following setting variables:

```scss
// {Boolean} - If you need styles for a wrapping container element, set this to true
$gr-set-container-style: false !default;

// {Boolean} - Defines if the container should be centered to the screen when the container max width is reached
$gr-center-container: false !default;

// ==============
// Completly customizable class names:
// ==============

// {String} - The prefix for all classes (.#{$gr-namespace-class}-*)
$gr-namespace-class: 'gr' !default;

// {String} - The class name for the container-selector: (.#{$gr-namespace-class}-#{$gr-container-class} default: .gr-container)
$gr-container-class: 'container' !default;

// {String} - The class name for the row-selector: (.#{$gr-namespace-class}-#{$gr-row-class} default: .gr-row)
$gr-row-class: 'row' !default;

// {String} - If you want to apply grid styles only for a specific breakpoint, this string divides class name and breakpoint identifier (.#{$gr-namespace-class}-12#{$gr-breakpoint-class-divider}#{breakpoint-name} e.g.: .gr-12@s)
$gr-breakpoint-class-divider: '\\@' !default;
```

#### Grid without breakpoints

If you don't maintain breakpoints you can use the following variables to define the grid behaviour:

```scss
// ==============
// General grid definitions
// ==============

// {Number} - Number of columns to use
$gr-columns: 12 !default;

// {Number} - Space between one column and another
$gr-gutter: 24px !default;

// {Number} - The maximum width of the wrapping container (if in use)
$gr-container-max-width: 1200px !default;

// {Number} - Spacing around the grid
$gr-container-padding: 20px !default;
```

#### Grid with breakpoint definitions

Breakpoints are boundary definitions for layout and/or other style changes. They are defined by media queries within the CSS and enable you to adjust the appearance of your site according to the needs of the users. For example they are useful, if the relative width of a module should change depending on  screen size (100% width on screens smaller than 1001px and 50% on wider screens). They may also come in handy when adjusting the font size to ensure readability and accessibility on the page.

When maintaining multiple breakpoints the grid needs to be configured for each of them. The variable `$gr-breakpoints` maps each breakpoint to a set of grid definitions. You can define as many breakpoints as you need and name them as you wish. The names will be used as an extension to the class names of all grid components and features. The `min` and `max` values specify the bounds (screen widths) of the current breakpoint:

ℹ️ The `min` and `max` values will be omitted inside the media query if the value is set to `null`. This is useful for the narrowest and especially for the widest breakpoint to cover all possible screen sizes. In addition `min` values should always be 1px bigger than the previous breakpoint's `max` value.

⚠️ Please avoid overlapping or undefined screen width ranges within your breakpoint definitions.

```scss
// {Map} - Contains the definitions for every breakpoint
$gr-breakpoints: (
  // {Map} - The key is used as a breakpoint identifier (breakpoint-name) and has to be unique
  s: (
    // {Number|null} - Minimum screen width for this breakpoint
    min: null,
    // {Number|null} - Maximum screen width for this breakpoint
    max: 600px,
    // {Number} - Space between columns
    gutter: 12px,
    // {Number} - Number of columns to use (how many columns have to fit in one row)
    columns: 12,
    // {Map} - Wrapping container definitions
    container: (
      // {Number|null} - The maximum width
      max-width: 100%,
      // {Number|null} - Container padding
      padding: 30px
    )
  ),
  m: (
    min: 601px,
    max: 1000px,
    gutter: 24px,
    columns: 12,
    container: (
      max-width: 768px,
      padding: 20px
    )
  ),
  l: (
    min: 1001px,
    max: null,
    gutter: 40px,
    columns: 12,
    container: (
      max-width: 1200px,
      padding: 60px
    )
  )
);
```

### Class Generation

After configuring your grid, you can let gr.scss generate classes for you:
```scss
@include gr-build-classes();
```
By default this will only generate the bare minimum of the grid. Each additional [feature](#features) can be enabled by a config map sent to the `gr-build-classes` function:

```scss
// {Map} - Map of feature identifiers pointing to booleans
// Set the boolean to true to enable the feature and to generate classes for it
@include gr-build-classes((
  prefix: false,
  suffix: false,
  push: false,
  pull: false,
  row-full: false,
  row-column: false,
  row-reverse: false,
  row-column-reverse: false,
  hide-show: false,
  order: false,
  align: false,
  no-gutter: false
));

// Example: Build the grid including classes for the prefix and suffix features
@include gr-build-classes((
  prefix: true,
  suffix: true
));
```

## Classes

### Base

#### Container

Wrap rows in containers to add a width constraint and screen padding to your grid:

ℹ️ This feature is optional and needs to be enabled before calling `gr-build-classes` to make it available:

```scss
$gr-set-container-style: true;

// Set to true if the container should be centered to the parent
$gr-center-container: true;
```

Available classes:

- `gr-container`
- `gr-container@{{breakpoint-name}}`

Markup example:
```html
<div class="gr-container">
  <!-- Your grid -->
</div>

<div class="gr-container@s">
  <!-- Your grid -->
</div>
```
#### Row

Embed columns to ensure the correct alignment and behaviour.

Available classes:

- `gr-row`
- `gr-row@{{breakpoint-name}}`


Markup example:
```html
<div class="gr-container">
  <div class="gr-row">
    <!-- Your columns -->
  </div>
</div>

<div class="gr-container@s">
  <div class="gr-row@s">
    <!-- Your columns -->
  </div>
</div>
```

#### Columns

The actual grid elements containing your content. The column count indicates how wide the column will be and is calculated like so: 100% / (breakpoint column count) * (column count).

Available classes:

- `gr-{{column-count}}`
- `gr-{{column-count}}@{{breakpoint-name}}`

Markup example:
```html
<div class="gr-container">
  <div class="gr-row">
    <div class="gr-12">
      <!-- Your content -->
    </div>
    <div class="gr-4 gr-12@s">
      <!-- Your content -->
    </div>
    <div class="gr-4 gr-6@s">
      <!-- Your content -->
    </div>
    <div class="gr-4 gr-6@s">
      <!-- Your content -->
    </div>
</div>
```

### Features

#### row-full

If you have a container you can force a row to push itself out of the containers bounds.

⚠️ The container has to be centered, for this feature to work properly.

Activate:
```scss
@include gr-build-classes((
  row-full: true
));
```

Available classes:

- `gr-row--full`
- `gr-row--full@{{breakpoint-name}}`

Markup example:
```html
<div class="gr-container">
  <div class="gr-row gr-row--full">
    <div class="gr-4">
      <!-- Your content -->
    </div>
    <div class="gr-4">
      <!-- Your content -->
    </div>
    <div class="gr-4">
      <!-- Your content -->
    </div>
  </div>
</div>
```

#### row-column

With this feature you can arrange the columns vertically independently of their width.

Activate:
```scss
@include gr-build-classes((
  row-column: true
));
```

Available classes:

- `gr-row--column`
- `gr-row--column@{{breakpoint-name}}`

Markup example:
```html
<div class="gr-row gr-row--column">
  <div class="gr-4">
    <!-- Your content -->
  </div>
  <div class="gr-4">
    <!-- Your content -->
  </div>
  <div class="gr-4">
    <!-- Your content -->
  </div>
</div>
```

#### row-reverse

This feature allows you to reverse the visible order of the columns within a row.

Activate:
```scss
@include gr-build-classes((
  row-reverse: true
));
```

Available classes:

- `gr-row--reverse`
- `gr-row--reverse@{{breakpoint-name}}`

Markup example:
```html
<div class="gr-row gr-row--reverse">
  <div class="gr-4">
    <!-- Displayed as third item -->
  </div>
  <div class="gr-4">
    <!-- Displayed as second item -->
  </div>
  <div class="gr-4">
    <!-- Displayed as first item -->
  </div>
</div>
```

#### row-column-reverse

In addition to the [row-reverse](#row-reverse) feature you can use column-reverse to reverse the visible order within a row but have the columns arranged vertically.

Activate:
```scss
@include gr-build-classes((
  row-column-reverse: true
));
```

Available classes:

- `gr-row--column-reverse`
- `gr-row--column-reverse@{{breakpoint-name}}`

Markup example:
```html
<div class="gr-row gr-row--column-reverse">
  <div class="gr-4">
    <!-- Displayed as third item -->
  </div>
  <div class="gr-4">
    <!-- Displayed as second item -->
  </div>
  <div class="gr-4">
    <!-- Displayed as first item -->
  </div>
</div>
```

#### prefix

By applying a prefix, you can add space to the left side, without having empty columns.

ℹ️ If you have set a prefix class without a specific breakpoint name the prefix applies to all screen resolutions. If you need to remove the prefix for a certain breakpoint you can use the `gr--prefix-0@{{breakpoint-name}}` class to override the default. By that you don't have to specify prefix class names for all breakpoints.

Activate:
```scss
@include gr-build-classes((
  prefix: true
));
```

Available classes:

- `gr--prefix-{{column-count}}`
- `gr--prefix-0@{{breakpoint-name}}`
- `gr--prefix-{{column-count}}@{{breakpoint-name}}`

Markup example:
```html
<div class="gr-row">
  <div class="gr-4 gr--prefix-3 gr--prefix-0@m">
    <!-- Your content -->
  </div>
  <div class="gr-4 gr--prefix-1 gr--prefix-0@m">
    <!-- Your content -->
  </div>
  <div class="gr-4 gr--prefix-8 gr--prefix-0@m">
    <!-- Your content -->
  </div>
</div>
```

#### suffix

By applying a suffix, you can add space to the right side, without having empty colums.

ℹ️ If you have set an suffix class without a specific breakpoint-name the suffix applies for all screen resolutions. If you need to remove the suffix for a certain breakpoint you can use the `gr--suffix-0@{{breakpoint-name}}` class to override the default. By that you don't have to specify suffix class names for all breakpoints.

Activate:
```scss
@include gr-build-classes((
  suffix: true
));
```

Available classes:

- `gr--suffix-{{column-count}}`
- `gr--suffix-0@{{breakpoint-name}}`
- `gr--suffix-{{column-count}}@{{breakpoint-name}}`

Markup example:
```html
<div class="gr-row">
  <div class="gr-4 gr--suffix-2 gr--suffix-0@m">
    <!-- Your content -->
  </div>
  <div class="gr-4 gr--suffix-1 gr--suffix-0@m">
    <!-- Your content -->
  </div>
  <div class="gr-2">
    <!-- Your content -->
  </div>
</div>
```

#### push

The push feature is used to translate a grid element to the right. In contrast to a prefix a push doesn't effect the position of other grid elements (It gets translated to the right without forcing the following elements to adjust their position accordingly):

Activate:
```scss
@include gr-build-classes((
  push: true
));
```

Available classes:

- `gr--push-{{column-count}}`
- `gr--push-0@{{breakpoint-name}}`
- `gr--push-{{column-count}}@{{breakpoint-name}}`

Markup example:
```html
<div class="gr-row">
  <!-- Gets pushed to the position of the second element -->
  <div class="gr-4 gr--push-4">
    <!-- Your content -->
  </div>
  <!-- Gets pulled to the position of the first element -->
  <div class="gr-4 gr--pull-4">
    <!-- Your content -->
  </div>
  <!-- Gets pushed by the width of two columns and lands outside of the grid bounds -->
  <div class="gr-4 gr--push-2@m">
    <!-- Your content -->
  </div>
</div>
```

#### pull

The pull feature is used to translate a grid element to the left. In contrast to a suffix a pull doesn't effect the postion of other grid elements (It gets translated to the left instead of forcing the following elements to make the room):

Activate:
```scss
@include gr-build-classes((
  pull: true
));
```

Available classes:

- `gr--pull-{{column-count}}`
- `gr--pull-0@{{breakpoint-name}}`
- `gr--pull-{{column-count}}@{{breakpoint-name}}`

Markup example:
```html
<div class="gr-row">
  <!-- Gets pushed to the position of the second element -->
  <div class="gr-4 gr--push-4">
    <!-- Your content -->
  </div>
  <!-- Gets pulled to the position of the first element -->
  <div class="gr-4 gr--pull-4">
    <!-- Your content -->
  </div>
  <!-- Gets pulled by the width of two columns and overlaps the previous element -->
  <div class="gr-4 gr--pull-2@m">
    <!-- Your content -->
  </div>
</div>
```

#### align

The align feature is used to define the horizontal and vertical alignment of the grid elements within the current row. They can be positioned top, right, bottom, left and center or even add space between or around the items:

ℹ️ Horizontal and vertical alignment can be used together.

Activate:
```scss
@include gr-build-classes((
  align: true
));
```

Available classes:

Vertically align:

Top (`align-items: flex-start;`):

- `gr-row--align-top`
- `gr-row--align-top@{{breakpoint-name}}`

Center (`align-items: center;`):

- `gr-row--align-middle`
- `gr-row--align-middle@{{breakpoint-name}}`

Bottom (`align-items: flex-end;`):

- `gr-row--align-bottom`
- `gr-row--align-bottom@{{breakpoint-name}}`

Horizontally align:

Start (`justify-content: flex-start;`):

- `gr-row--align-start`
- `gr-row--align-start@{{breakpoint-name}}`

Center (`justify-content: center;`):

- `gr-row--align-center`
- `gr-row--align-center@{{breakpoint-name}}`

End (`justify-content: flex-end;`):

- `gr-row--align-end`
- `gr-row--align-end@{{breakpoint-name}}`

Space around all elements (`justify-content: space-around;`):

- `gr-row--align-around`
- `gr-row--align-around@{{breakpoint-name}}`

Space between elements (`justify-content: space-between;`):

- `gr-row--align-between`
- `gr-row--align-between@{{breakpoint-name}}`

Markup example:
```html
<div class="gr-row gr-row--align-between gr-row--align-middle">
  <div class="gr-2">
    <!-- Your content -->
  </div>
  <div class="gr-2">
    <!-- Your content -->
  </div>
  <div class="gr-2">
    <!-- Your content -->
  </div>
</div>
```

#### hide-show

The hide-show feature can be used to make elements visible or invisible either by setting the display or the visibility property:

Activate:
```scss
@include gr-build-classes((
  hide-show: true
));
```

Available classes:

`display: none;`

- `gr--hide`
- `gr--hide@{{breakpoint-name}}`

`display: block;`

- `gr--show`
- `gr--show@{{breakpoint-name}}`

`visibility: hidden;`

- `gr--hidden`
- `gr--hidden@{{breakpoint-name}}`

`visibility: visible;`

- `gr--visible`
- `gr--visible@{{breakpoint-name}}`

Markup example:
```html
<div class="gr-row">
  <div class="gr-12 gr--hide gr--show@m">
    <!-- Your content - visible at m -->
  </div>
  <div class="gr-12 gr--hide gr--show@l">
    <!-- Your content - visible at l -->
  </div>
  <div class="gr-12 gr--hide@m gr--hide@l">
    <!-- Your content - visible at s -->
  </div>
</div>
```

#### no-gutter

The no-gutter feature enables you to completely or partially remove the gutter. To do so, you can remove the negative margin on the row and the paddings on the grid elements as needed.

Activate:
```scss
@include gr-build-classes((
  no-gutter: true
));
```

Available classes:

row:

- `gr-row--no-gutter`
- `gr-row--no-gutter@{{breakpoint-name}}`
- `gr-row--no-gutter-left`
- `gr-row--no-gutter-left@{{breakpoint-name}}`
- `gr-row--no-gutter-right`
- `gr-row--no-gutter-right@{{breakpoint-name}}`

column:

- `gr--no-gutter`
- `gr--no-gutter@{{breakpoint-name}}`
- `gr--no-gutter-left`
- `gr--no-gutter-left@{{breakpoint-name}}`
- `gr--no-gutter-right`
- `gr--no-gutter-right@{{breakpoint-name}}`

Markup example:
```html
<!-- Completly remove gutter -->
<div class="gr-row gr-row--no-gutter">
  <div class="gr-4 gr--no-gutter">
    <!-- Your content -->
  </div>
  <div class="gr-4 gr--no-gutter">
    <!-- Your content -->
  </div>
  <div class="gr-4 gr--no-gutter">
    <!-- Your content -->
  </div>
</div>

<!-- Or highly customized -->

<div class="gr-row gr-row--no-gutter-left gr-row--no-gutter@l">
  <div class="gr-4 gr--no-gutter-left gr--no-gutter-right@m gr--no-gutter-right@l">
    <!-- Your content -->
  </div>
  <div class="gr-4 gr--no-gutter@m gr--no-gutter@l">
    <!-- Your content -->
  </div>
  <div class="gr-4 gr--no-gutter@l">
    <!-- Your content -->
  </div>
</div>
```

#### order

The order feature is used to reorder elements without changing the markup structure.

ℹ️ `gr--order-first` will put the element at the very first position even if you have defined an element with `gr--order-1`. `gr--order-last` will put the element at the very end of a list of grid elements.

Activate:
```scss
@include gr-build-classes((
  order: true
));
```

Available classes:

- `gr--order-first`
- `gr--order-first@{{breakpoint-name}}`
- `gr--order-last`
- `gr--order-last@{{breakpoint-name}}`
- `gr--order-{{column-count}}`
- `gr--order-{{column-count}}@{{breakpoint-name}}`

Markup example:
```html
<div class="gr-row">
  <!-- 2. -->
  <div class="gr-2 gr--order-2">
    <!-- Your content -->
  </div>
  <!-- 5. -->
  <div class="gr-2 gr--order-5">
    <!-- Your content -->
  </div>
  <!-- 1. -->
  <div class="gr-2 gr--order-first">
    <!-- Your content -->
  </div>
  <!-- 6. -->
  <div class="gr-2 gr--order-last">
    <!-- Your content -->
  </div>
  <!-- 4. -->
  <div class="gr-2 gr--order-4">
    <!-- Your content -->
  </div>
  <!-- 3. -->
  <div class="gr-2 gr--order-3">
    <!-- Your content -->
  </div>
</div>
```

## Helpers

gr.scss provides some useful helpers for you to work with your defined breakpoints.

### gr-media

By using the `gr-media` mixin, you can set styles within the breakpoints of your grid, without redefining the media queries over and over again. It receives one or two breakpoint identifiers, defined in your [grid settings](#grid-with-breakpoint-definitions). Calling it with one breakpoint identifier `gr-media` sets the minimum and maximum value by the definitions of this single breakpoint. Otherwise the mixin uses the first breakpoint to get the minimum and the second to get the maximum value, if it is set.

Usage:
```scss
@include gr-media(breakpoint-name1[, breakpoint-name2]) {
  // Your style definitions
}
```

Examples:

Breakpoint with defined min and max value:

```scss
@include gr-media(m) { ... }

// CSS output:
@media (min-width: 601px) and (max-width: 1000px) { ... }
```
Breakpoint with min value of null:

```scss
@include gr-media(s) { ... }

// CSS output:
@media (max-width: 600px) { ... }
```
One breakpoint with defined min value and the a second breakpoint with a defined max value:

```scss
@include gr-media(m, l) { ... }

// CSS output:
@media (min-width: 601px) and (max-width: 1200px) { ... }
```
Onebreakpoint with defined min value and the a second breakpoint with max value set to null:

```scss
@include gr-media(m, xl) { ... }

// CSS output:
@media (min-width: 601px) { ... }
```
### gr-get-value

Sometimes it comes in handy, to fetch grid facts for specific breakpoints to apply them to non grid elements. You can use the `gr-get-value` function for this very reason. You need to provide the breakpoint identifier followed by the keys to describe the nesting path to the value you want to get access to.

Usage:

```scss
.some-selector {
  property: gr-get-value(breakpoint-name, key1[, key2[, ...[, keyN]]]);
}

```
Examples:

Access the first level of a single breakpoint's map:
```scss
.tile {
  padding-left: gr-get-value(s, gutter) / 2;
}

// CSS output:
.tile {
  padding-left: 6px;
}
```
Access a deeper nested level of a breakpoint::
```scss
.custom-container {
  max-width: gr-get-value(l, container, max-width);
}

// CSS output:
.custom-container {
  max-width: 1200px;
}
```
Access values for each breakpoint and wrap them in a media query condition related to that breakpoint:
```scss
@each $breakpoint, $breakpoint-config in $gr-breakpoints {
  @include gr-media($breakpoint) {
    .tile {
      padding-left: gr-get-value($breakpoint, gutter) / 2;
    }
  }
}
```

## Browser Support

| ![](https://raw.githubusercontent.com/godban/browsers-support-badges/master/src/images/edge.png)<br />IE11 / Edge | ![](https://raw.githubusercontent.com/godban/browsers-support-badges/master/src/images/firefox.png)<br />Firefox | ![](https://raw.githubusercontent.com/godban/browsers-support-badges/master/src/images/chrome.png)<br />Chrome | ![](https://raw.githubusercontent.com/godban/browsers-support-badges/master/src/images/safari.png)<br />Safari | ![](https://raw.githubusercontent.com/godban/browsers-support-badges/master/src/images/safari-ios.png)<br />iOS Safari | ![](https://raw.githubusercontent.com/godban/browsers-support-badges/master/src/images/chrome-android.png)<br />Chrome for Android |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | :----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| IE11 / Edge                                                  | last 2 versions                                              | last 2 versions                                              | last 2 versions                                              | last 2 versions                                              | last 2 versions                                              |
