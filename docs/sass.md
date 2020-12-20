# Sass Coding Guidelines

## Table of contents

- [Preliminaries](#preliminaries)
  - [General guide](#general-guide)
  - [General resource](#general-resource)
- [Specificity](#specificity)
  - [Performance](#performance)
- [Formatting](#formatting)
  - [Indentation](#indentation)
  - [Documenting](#documenting)
  - [Commenting](#commenting)
  - [Spacing](#spacing)
  - [Quotes](#quotes)
  - [Value Declaration](#value-declaration)
  - [Declaration Order](#declaration-order)
- [Pseudo Elements and Classes](#pseudo-elements-and-classes)
- [Units](#units)
- [Nesting](#nesting)
- [@extend or @inlcude](#extend-or-include)
- [Components](#components)
  - [componentName](#componentName)
  - [componentName--modifierName](#componentName--modifierName)
  - [componentName-descendantName](#componentName-descendantName)
- [Utilities](#utilities)
  - [u-utilityName](#u-utilityName)
- [Variables and Mixins](#variables-and-mixins)
  - [Variables](#variables)
  - [Component / Micro app variables](#component-micro-app-variables)
  - [Maps](#maps)
  - [colors](#colors)
  - [z-index](#zindex)
  - [font-weight](#fontweight)
  - [line-height](#lineheight)
  - [Animations](#animations)
  - [Mixins](#mixins)
- [Polyfills](#polyfills)
- [JavaScript](#javascript)
- [Folder Structure](#folders)

## Preliminaries

[include use BEM info, 2 spaces indentation, use em /rem, dont use colors aned capital for colors, edit performance+spacing,quotes(bad), polyfill with autoprefixer, cleancss, purify, Refer to PostCSS: write scss->compile css-> clean /minify-> purify(remove unused class)-> polyfill/autoprefix ->, fix/delete unused links ]

Strictly adhere to the agreed-upon style guide listed below. The general
principle is to develop DRY (Don't Repeat Yourself) SCSS, built around reusable
components and patterns.

- All code should look like a single person has typed it.
- Don't try to prematurely optimize your code; keep it readable and understandable.
- Break down complex components until they are made up of simple components.
- Save your complex components as patterns so they can be easily reused.
- Build your component as a mixin which outputs _optional_ css.

### General guide

### General resource

Here are some resources that might be useful to get a quick understanding of the coding standard and tools that we are using:

1. BEM naming convention: https://en.bem.info/
2. Sass guidelines: https://sass-guidelin.es/
3. MDN HTML guidelines: https://developer.mozilla.org/en-US/docs/MDN/Guidelines/Code_guidelines/HTML

## Specificity

On large code bases, it's preferable and a tonne more maintainable if the
specificity of selectors are all as equal and as low as humanly possible.

> 👍 GOOD 😊
> Use classes in your SCSS for styling.

```css
.component {
  ...;
}
```

> 👎 BAD 😱
> Use ID's for styling.

```css
#component {
  ...;
}
```

> 👍 GOOD 😊
> Style the base elements (such as typography elements).

```css
h1 {
  ...;
}
```

> 👎 BAD 😱
> Reference or style descendent elements in your class selectors.

```css
.component h1 {
  ...;
}
```

> 👎 BAD 😱
> Use overqualified selectors in your CSS. Do not prepend a class or ID with an element.

```css
div.container {
  ...;
}
```

#### Performance

Overly specific selectors can also cause performance issues. Consider:

```css
ul.user-list li span a:hover {
  color: red;
}
```

Selectors are resolved right to left, exiting when it has been detected the
selector does not match. This requires a lot of DOM walking and for large
documents can cause a significant increase in the layout time.

If we know we want to give all `a` elements inside the `.user-list` red on
hover we can simplify this style using [BEM standards](#http://getbem.com/introduction/):

```css
.user-list-link:hover {
  color: red;
}
```

## Formatting

The following are some high level page formatting style rules.

- Remove all trailing white-space from your file, Sublime Text can even do this upon saving.
  - Tip: set your editor to show white-space.
- Leave one clear line at the bottom of your file.

#### Indentation

- Use a soft-tab of 2 spaces.
- Use white-space to improve readability.
- Feel free to use indentation to show hierarchy.

> 👍 GOOD 😊

```css
.component {
  ...;
}

.component__child {
  ...;
}

.component__childSecond {
  ...;
}
```

#### Documenting

Every variable, function, mixin and placeholder that is intended to be reused all over the codebase should be documented as part of the global API using [SassDoc](#http://sassdoc.com).

```css
/// Vertical rhythm baseline used all over the code base.
/// @type Length
$vertical-rhythm-baseline: 1.5rem;
```

> Note: Three slashes (/) required.

SassDoc has two major roles:

- forcing standardized comments using an annotation-based system for everything that is part of a public or private API;
- being able to generate an HTML version of the API documentation by using any of the SassDoc endpoints (CLI tool, Grunt, Gulp, Broccoli, Node…).

> 👍 GOOD 😊

```css
/// Mixin helping defining both `width` and `height` simultaneously.
///
/// @author Hugo “Kitty” Giraudel
///
/// @access public
///
/// @param {Length} $width - Element’s `width`
/// @param {Length} $height [$width] - Element’s `height`
///
/// @example scss - Usage
///   .foo {
///     @include size(10em);
///   }
///
///   .bar {
///     @include size(100%, 10em);
///   }
///
/// @example css - CSS output
///   .foo {
///     width: 10em;
///     height: 10em;
///   }
///
///   .bar {
///     width: 100%;
///     height: 10em;
///   }
@mixin size($width, $height: $width) {
  width: $width;
  height: $height;
}
```

#### Commenting

- Separate your code into logical sections using standard comment blocks.
- Leave one clear line under your section comments.
- Leave two clear lines above comment blocks.
- Annotate your code inside a comment block, leaving a reference # next to the line.

> 👍 GOOD 😊

```css
// =============================================================================
// FILE TITLE / SECTION TITLE
// =============================================================================

// Comment Block / Sub-section
// -----------------------------------------------------------------------------
//
// Purpose: This will describe when this component should be used. This comment
// block is 80 chars long
//
// 1. Mark lines of code with numbers which are explained here.
// This keeps your code clean, while also allowing detailed comments.
//
// -----------------------------------------------------------------------------

.component {
  ... // 1;
}
```

#### Spacing

- CSS rules should be comma separated but live on new lines.
- Include a single space before the opening brace of a rule-set.
- Include a single space after the colon of a declaration.
- Include a semi-colon at the end of every declaration in a declaration block.
- Include a space after each comma in comma-separated property or function values.
- Place the closing brace of a rule-set on its own line.
- CSS blocks should be separated by a single clear line.

> 👍 GOOD 😊

```css
.content,
.content--featured {
  padding: 0;
  margin: 0;
  font-family: "Helvetica", sans-serif;
}

.newSection {
  ...;
}

.newSection--featured {
  ...;
}
```

> 👎 BAD 😱 [make class one line]

```css
.content,
.content--featured {
  padding: 0;
  margin: 0;
  font-family: "Helvetica", sans-serif;
}
.newSection {
  ...;
}
.newSection--featured {
  ...;
}
```

#### Quotes

> 👍 GOOD 😊
> Always use double quotes when available.
> Quote attribute values in selectors

```css
input[type="checkbox"] {
  background-image: url("/img/you.jpg");
  font-family: "Helvetica Neue Light", "Helvetica Neue", Helvetica, Arial;
}
```

> 👎 BAD 😱 [remove all double quotes]

```css
input[type="checkbox"] {
  background-image: url(/img/you.jpg);
  font-family: Helvetica Neue Light, Helvetica Neue, Helvetica, Arial;
}
```

#### When declaring values

- Use lower-case and shorthand hex values
- Use unit-less line-height values
- Where allowed, avoid specifying units for zero values
- Never specify the height property unless it's specifically needed (`min-height` is cool)
- Never use `!important` (Utility classes are an exception but still should be
  avoided)
- Try to only style the property you are explicitly concerned with to reduce
  over zealously resetting something you might want to inherit
  - `background-color: #333` over `background: #333`
  - `margin-top: 0.5rem` over `margin: 0.5rem 0 0`
  - Use shorthand if you can, be sensible

> 👍 GOOD 😊

```css
.component {
  background-color: #ccc;
  color: #aaa;
  left: 0;
  line-height: 1.25;
  min-height: 4rem;
  padding: 0 2rem;
  top: 0;
}
```

> 👎 BAD 😱

```css
.component {
  background: #ccc;
  color: #aaaaaa;
  left: 0rem;
  line-height: 24px;
  height: 4rem !important;
  padding: 0rem 2rem 0rem 2rem;
  top: 0rem;
}
```

#### Declaration order

There are a millions opinions and thoughts on logical ordering and grouping.
Don't force someone to learn your opinion, ordering doesn't matter, consistency
does. Just use the alphabet, _everyone_ knows it.

- @extend
- @include
- styling codes

> 👍 GOOD 😊

```css
.component {
  @extend %a-placeholder;
  @include silly-links;
  color: #aaa;
  left: 0;
  line-height: 1.25;
  min-height: 4rem;
  padding: 0 2rem;
  top: 0;
  width: 75%;
}
```

> 👎 BAD 😱

```css
.component {
  min-height: 4rem;
  left: 0;
  @include silly-links;
  top: 0;
  width: 75%;
  color: #aaa;
  @extend %a-placeholder;
  line-height: 1.25;
  width: 75%;
  padding: 0 2rem;
}
```

## Pseudo Elements and Classes

Pseudo elements and classes are very different things, as is the syntax used to
declare them. Declare pseudo _**classes**_ with a single colon. Declare pseudo
_**elements**_ with a double colon.

> 👍 GOOD 😊

```css
.component:focus {
  ...;
}

.component:hover {
  ...;
}

.component::before {
  ...;
}

.component::after {
  ...;
}
```

> 👎 BAD 😱

```css
.component:after {
  ...;
}
```

## Units

For units, the rules are as follows:

> 👍 GOOD 😊

- Use `rem` units as primary unit type. This includes:
  - Positioning (`top`, `right`, `bottom`, `left`)
  - Dimensions (Such as `width`, `height`, `margin`, `padding`)
  - Font size
- Use `px` units as primary unit type for the following properties:
  - Border widths (`border: 1px solid #bada55;`)
- Use `%` units only if necessary, where `rem` will not suffice:
  - Positioning (`top`, `right`, `bottom`, `left`)
  - Dimensions (`width`, `height`)
- Line-height should be kept unit-less. If you find you're using a line-height
  with a set unit type, try to think of alternative ways to achieve the same outcome.
  If it's a unique case which requires a specific `px` or `rem` unit, outline the
  reasoning with comments so that others are aware of its purpose.

> 👎 BAD 😱

- Avoid all use of magic numbers. (`margin-top: 3.78rem;`)

These rules are adopted based on this [guidelines](#https://medium.com/@kilgarenone/in-short-when-to-use-px-em-and-rem-b2de94e1b829)

## Nesting

Nesting is handy, _sometimes_, but it will quickly conflict with the [Specificty](#specificity) and [Performance](#performance) guidelines.

As we follow conventions and thoughts from popular and widely accepted
methodologies such as BEM, the use of the Suffix can help immensely though.

- Watch your output, be mindful of [Specificty](#specificity) and
  [Performance](#performance)
- Aim for a maximum depth of just 1 nested rule

> 👍 GOOD 😊

```css
.panel__body {
  position: relative;
}

.panel__sideBar {
  z-index: 10;
}

.panel__sideBar-select {
  cursor: pointer;
}

.panel__sideBar--label {
  color: #aeaeae;

  &.has-smallFont {
    font-size: 1rem;
  }
}
```

At its worst, this produces:

```css
.panel__sideBar--label .has-smallFont {
  font-size: 1rem;
}
```

> 👎 BAD 😱

```css
.bc-tab-panel {
  .panel-body {
    position: relative;
    & .panel-side-bar {
      z-index: 10;
      & .panel-side-item {
        cursor: pointer;
        & .panel-side-item-label {
          color: #aeaeae;

          &.small-font {
            font-size: 1rem;
          }
        }
      }
    }
  }
}
```

At it's worst, this produces:

```css
.bc-tab-panel
  .panel-body
  .panel-side-bar
  .panel-side-item
  .panel-side-item-label.small-font {
  font-size: 1rem;
}
```

## @extend or @include

- Excessive use of `@include` can cause unnecessary bloat to your stylesheet
- Excessive use of `@extend` can create large selector blocks (not helpful in web inspector)
  and hoisting of your selector can cause override and inheritance issues.
- We advise to `@include` over `@extend` generally, but use common sense. In situations where it's better to `@extend` it's safer to do so on a placeholder selector.

> 👍 GOOD 😊
> Make use of placeholder selectors to separate repeated local styles

```css
%placeholderSelector {
  background-color: #eee;
}

.component1 {
  @extend %placeholderSelector;
  color: red;
}

.component2 {
  @extend %placeholderSelector;
  color: blue;
}
```

## Components

Syntax: `blockName__elementName--modifierName`

This component syntax is mainly taken from [BEM/Block Element Modifier](#http://getbem.com/introduction/). However, instead of using the dashed-name, we use camelCase for the components name here.

You can think of components as custom elements that enclose specific semantics,
styling, and behaviour.

**Do not choose a class name based on its visual presentation or its content.**

The primary architectural division is between components and utilities:

- componentName (eg. `.dropdown` or `.buttonGroup`)
- componentName--modifierName (eg. `.dropdown--dropUp` or `.button--primary`)
- componentName__elementName (eg. `.dropdown__item`)
- u-utilityName (eg. `.u-textTruncate`)

#### Block name

The component's name must be written in camel case.

```css
.myComponent {
  /* ... */
}
```

#### Modifier name

A component modifier is a class that modifies the presentation of the base
component in some form. Modifier names must be written in camel case and be
separated from the component name by **two** hyphens. The class should be included
in the HTML _in addition_ to the base component class.

```css
/* Core button */
.button {
  ...;
}

.button--primary {
  ...;
}
```

```html
<button class="button button--primary">...</button>
```

#### Element name

A component element is a class that is attached to a descendant node of a
component. It's responsible for applying presentation directly to the descendant
on behalf of a particular component. Element names must be written in camel case.

```html
<article class="tweet">
  <header class="tweet__header">
    <img class="tweet__avatar" src="{$src}" alt="{$alt}" />
    ...
  </header>
  <div class="tweet__body">...</div>
</article>
```

You might notice that `tweet-avatar`, despite being a descendant of `tweet-header`
does not have the class of `tweet-header-avatar`. Why? Because it doesn't necessarily
**have** to live there. It could be adjacent to `tweet-header` and function the same
way. Therefore, you should **only** prepend a descendant with its parent if must
live there. Strive to keep class names as short as possible, but as long as necessary.

When building a component, you'll often run into the situation where you have a
list, group or simply require a container for some descendants. In this case, it's
much better to follow a pattern of pluralising the container and having each
descendant be singular. This keeps the relationship clear between descendant levels.

> 👍 GOOD 😊

```html
<nav class="pagination">
  <ul class="pagination-list">
    <li class="pagination-listItem">...</li>
  </ul>
</nav>
```

```html
<ul class="breadcrumbs">
  <li class="breadcrumb">
    <a class="breadcrumb-label" href="#"></a>
  </li>
</ul>
```

> 👎 BAD 😱
> Avoid verbose descendant names

```html
<nav class="pagination">
  <ul class="pagination-pages">
    <li class="pagination-pages-page">...</li>
  </ul>
</nav>
```

```html
<ul class="breadcrumbs">
  <li class="breadcrumbs-breadcrumb">
    <a class="breadcrumbs-breadcrumb-label" href="#"></a>
  </li>
</ul>
```

## Utilities

Utility classes are low-level structural and positional traits. Utilities can
be applied directly to any element; multiple utilities can be used together;
and utilities can be used alongside component classes.

Utility classes should be used sparingly, lean towards component level styling
to make for as reusable HTML patterns as possible.

#### u-utilityName

Syntax: `u-<utilityName>`

Utilities must use a camel case name, prefixed with a `u` namespace.

## Variables and Mixins

Variables and Mixins should follow similar naming conventions.

#### Variables

Syntax: `<componentName>[--modifierName][-elementName]`

Variables should be named as such, things that can change over time.

Variables should also follow our component naming convention to show context and be in camelCase. If the variable is a global, generic variable, the property name should be prefixed first, followed by the variant and or modifier name for clearer understanding of use.

> 👍 GOOD 😊
> Abstract your variable names

```CSS
$color-brandPrimary:  #aaa;
$fontSize:            1rem;
$fontSize--large:     2rem;
$lineHeight--small:   1.2;
```

> 👎 BAD 😱
> Name your variables after the color value

```css
$color-blue: #00ffee;
$color-lightBlue: #eeff00;
```

#### Maps

Variable maps with a simple getter mixin, can help simplify your variable names
when calling them, and help better group variables together using their
relationship.

> 👍 GOOD 😊

```scss
// Setting variables and mixin
// -----------------------------------------------------------------------------

$colors: (
  primary: (
    base: #00abc9,
    light: #daf1f6,
    dark: #12799a,
  ),
  secondary: (
    base: #424d55,
    light: #ccc,
    lightest: #efefef,
    dark: #404247,
  ),
  success: (
    base: #bbd33e,
    light: #eaf0c6,
  ),
);

@function color($color, $tone: "base") {
  @return map-get(map-get($colors, $color), $tone);
}
```

```scss
// Usage
// -----------------------------------------------------------------------------

body {
  color: color("secondary");
}

h1,
h2,
h3 {
  color: color("secondary", "dark");
}

.alert {
  background-color: color("primary", "light");
}
```

**Every variable used in the core architecture must be based off the global
variables.**

#### Mixins

Mixins follow regular camel case naming conventions and do not require namespacing. If you are creating a mixin for a utility, it will need to match the utility name (including `u` namespacing).

- `@mixin buttonVariant;`
- `@mixin u-textTruncate;`

## Polyfills

We try not to replicate CSS polyfills that auto-prefixer can
supply in a Grunt or Gulp task. This keeps our SCSS code base lean and future proof.

> 👍 GOOD 😊

```css
.button {
  border-radius: 2.5rem;
}
```

> 👎 BAD 😱
> Add vendor prefixes at all.

```css
.button {
  @include border-radius(2.5rem);
}
```

```css
.button {
  -ms-border-radius: 2.5rem;
  -o-border-radius: 2.5rem;
  -webkit-border-radius: 2.5rem;
  border-radius: 2.5rem;
}
```

## Folder Structure

#### General principle

The Sass folder structure we're proposing, will have two slight differences
between the core framework and micro apps, however the bulk of the structure is
identical between the two.

The idea is to have the least amount of folders as possible, but as many as we
need to define clear, structured patterns in your Sass.

#### Core Folder Structure

```
.
├── sass
|   ├── settings/
|   └── tools/
|   └── vendor/
|   └── components/
|   └── utilities/
```

**/settings:** Contains all of your SCSS variables for your framework. Within
this folder is 1 primary file `_settings.scss`, which imports all other variable
files that have been broken into logical files such as `_colors.scss`,
`_typography.scss`, `_z-index.scss` and your chosen frameworks variables, for
example `_foundation.scss`.

**/tools:** Contains all of your Sass mixins. Within this folder is 1 primary
file `_tools.scss`, which imports all other mixin files that have been broken into
logical files. No framework mixins should appear in this folder as they can be
consumed from their own respective `/vendor` or `/components` folder.

**/vendor:** Contains all of your vendor files, such as normalize, bootstrap,
foundation, animate.css, etc. All readily consumable third party files belong
here, and can be imported in the framework base file as required. No file in
the /vendor folder should ever be modified.

**/components:** Contains all of your components. This folder will make up the
vast majority of your compiled CSS. All custom components simply live inside
this folder, for example `/components/component/_component.scss`. It also contains
the consumed version of your chosen /vendor framework's components, which will be
reworked to adhere to the Naming Conventions and Style Guide. They will live
inside a subfolder of the framework's name, for example `/components/foundation/`.

**/utilities:** Contains all CSS snippets which can be applied to your HTML for
quick prototyping, or a case by case basis where a unique, yet repeatable style
is required. Every utility found within this folder will have both a class and a
mixin. An example being, truncatedText. You can utilise it by applying the class
`.u-truncatedText` or by applying a mixin, `@include truncatedText;`.

#### Micro App Folder Structure

```
.
├── sass
|   ├── settings/
|   └── tools/
|   └── vendor/
|   └── layouts/
|   └── components/
|   └── utilities/
|   └── shame/
```

There are only two minor differences in a micro app, when compared to the core
framework. Firstly you'll notice that the /framework folder has been replaced by
a /layouts folder, as well as the addition of the /shame folder.

**/layouts:** Contains your micro app "layouts" and page specific styling.
Essentially creating the wrapping sections and grids for your app, where the
core framework's components will live inside. For example, a layout file could
potentially be your micro app's navigation. It is important to note that the
styling for individual navigation items and all other inner components styling
do not live in the layout file. There purpose is purely for the containing
elements that set up your app.

**/shame:** This interestingly named folder has one goal: to remain empty. It's
purpose is the place for all of those hot fixes or quick hacks in throwing
something together. Any code which you don't feel is "complete" can also live
here. It creates clear visibility on less than perfect code, especially when it
comes to code reviews, and creates a trail for your dodgy code that if left
somewhere in your component/layout code could be forgotten about and left.

**A note on: /components & /utilities:** Within your micro app, these folders should
only house your app's unique code. Any repeatable component or utility that could
be re-used across other micro apps should be flagged and a PR opened for adding
it into the core framework.
