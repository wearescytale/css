# Scytale CSS / Sass Styleguide

Based on the awesome [Airbnb Style Guide](https://github.com/airbnb/css)

*A mostly reasonable approach to CSS and Sass*

## Table of Contents

  1. [Terminology](#terminology)
    - [1.1 Rule Declaration](#rule-declaration)
    - [1.2 Selectors](#selectors)
    - [1.3 Properties](#properties)
  2. [CSS](#css)
    - [2.1 Formatting](#formatting)
    - [2.2 Comments](#comments)
  3. [Sass](#sass)
    - [3.1 Syntax](#syntax)
    - [3.2 Ordering](#ordering-of-property-declarations)
    - [3.3 Mixins](#mixins)
    - [3.4 Placeholders](#placeholders)
    - [3.5 Nested selectors](#nested-selectors)
  4. [RSCSS](#rscss)
      - [4.1 Components](#components)
      - [4.2 Elements](#elements)
      - [4.3 Variants](#variants)
      - [4.4 Nested components](#nested-components)
      - [4.5 Layouts](#layouts)
  5. [Conventions](#conventions)

## Terminology

### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```sass\
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```sass
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```sass
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

## CSS

### Formatting

- [2.1.1](#2.1.1) <a name='2.1.1'></a> Use soft tabs (4 spaces) for indentation. This improves readability and keeps the space uniform

```sass
// bad
.avatar{
  border-radius: 50%;
}

// good
.avatar{
    border-radius: 50%;
}
```

- [2.1.2](#2.1.2) <a name='2.1.2'></a> Use classes for all selectors. Ids are reserved for E2E testing and data arguments are for JS libraries

```sass
// bad
#panel {
    /* ... */
}

[data-panel] {
    /* ... */
}

// good
.panel {
    /* ... */
}
```

- [2.1.3](#2.1.3) <a name='2.1.3'></a> Avoid using tag selectors. Tag selectors come at a small performance penalty

```sass
// bad
h3 {
    /* ... */
}

// good
.subtitle {
    /* ... */
}
```

- [2.1.4](#2.1.4) <a name='2.1.4'></a> When using multiple selectors in a rule declaration, give each selector its own line. This way its much easier to find selectors when scanning the document

```sass
// bad
.panel, .box {
    /* ... */
}

// good
.panel,
.box {
    /* ... */
}
```

- [2.1.5](#2.1.5) <a name='2.1.5'></a> Put a space before the opening brace `{` in rule declarations. In properties, put a space after, but not before, the `:` character. Put closing braces `}` of rule declarations on a new line. Put blank lines between rule declarations

```sass
// bad
.panel{
    font-size:2px;color:white}

// good
.panel {
    font-size: 2px;
    color: white;
}
```

### Comments

- [2.2.1](#2.2.1) <a name='2.2.1'></a> Prefer line comments `//` to block comments.

- [2.2.1](#2.2.1) <a name='2.2.1'></a> Prefer comments on their own line. Avoid end-of-line comments.

## Sass

### Syntax

- [3.1.1](#3.1.1) <a name='3.1.1'></a> Use the `.scss` syntax, never the original `.sass` syntax

- [3.1.2](#3.1.2) <a name='3.1.2'></a> Order your `@extend`, `@include` declarations and regular CSS lastly with a newline between the last two.

```scss
.btn-green {
    @extend %btn;
    @include transition(background 0.5s ease);

    background: green;
    font-weight: bold;
}
```

- [3.1.3](#3.1.3) <a name='3.1.3'></a> Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply [3.1.2](#3.1.2) as above to your nested selectors.

```scss
.btn {
  @extend %btn;

  background: green;
  font-weight: bold;

  @include transition(background 0.5s ease);

  .icon {
    margin-right: 10px;
  }
}
```

### Mixins

Mixins, defined via `@mixin` and called with `@include`, should be used sparingly and only when function arguments are necessary. A mixin without function arguments (i.e. `@mixin hide { display: none; }`) is better accomplished using a placeholder selector (see below) in order to prevent code duplication.

### Placeholders

Placeholders in Sass, defined via `%selector` and used with `@extend`, are a way of defining rule declarations that aren't automatically output in your compiled stylesheet. Instead, other selectors “inherit” from the placeholder, and the relevant selectors are copied to the point in the stylesheet where the placeholder is defined. This is best illustrated with the example below.

Placeholders are powerful but easy to abuse, especially when combined with nested selectors. **As a rule of thumb, avoid creating placeholders with nested rule declarations, or calling `@extend` inside nested selectors.** Placeholders are great for simple inheritance, but can easily result in the accidental creation of additional selectors without paying close attention to how and where they are used.

**Sass**

```sass
// Unless we call `@extend %icon` these properties won't be compiled!
%icon {
  font-family: "Airglyphs";
}

.icon-error {
  @extend %icon;
  color: red;
}

.icon-success {
  @extend %icon;
  color: green;
}
```

**Compiled CSS**

```sass
.icon-error,
.icon-success {
  font-family: "Airglyphs";
}

.icon-error {
  color: red;
}

.icon-success {
  color: green;
}
```

### Nested selectors

**Do not nest selectors more than three levels deep!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable

## [RSCSS](http://rscss.io/)

### Components

![Component Example](img/component-example.png)

- [4.1.1](#4.1.1) <a name='4.1.1'></a> Name components will be named with at least two works, seperated by a dash.

```sass
.like-button {
    /* ... */
}

.search-form {
    /* ... */
}
```
### Elements

![Element Example](img/component-elements.png)

- [4.2.1](#4.2.1) <a name='4.2.1'></a> Each component may have elements. They should have classes that are only one word.

```sass
.search-form {
    > .field {
        /* ... */
    }

    > .action {
        /* ... */
    }
}
```
- [4.2.2](#4.2.2) <a name='4.2.2'></a> Always prefer to use the `>` child selector whenever possible.

```sass
.article-card {
    .title {
        /* okay */
    }

    > .author {
        /* better */
    }
}
```

- [4.2.3](#4.2.3) <a name='4.2.3'></a> For elements that need two or more words, concatenate them without dashes or underscores.

```sass
.profile-box {
    > .firstname {
        /* ... */
    }

    > .lastname {
        /* ... */
    }
}
```

### Variants

![Variants Example](img/component-modifiers.png)

- [4.3.1](#4.3.1) <a name='4.3.1'></a> Variants customize a component or an element. Variants are prefixed by a dash `-`.

```sass
.search-form {
    &.-prefixed {
        /* ... */
    }

    &.-compact {
        /* ... */
    }
}
```

### Nested components

![Nested Example](img/component-nesting.png)

- [4.4.1](#4.4.1) <a name='4.4.1'></a> When a component may need to appear a certain way when nested in another component. Avoid modifying the nested component by reaching into it from the containing component. Instead, prefer to add a variant to the nested component and apply it from the containing component.

```sass

// bad
.article-header {
    > .vote-box > .up {
        /* ... */
    }
}

// good
.vote-box {
    &.-highlight > .up {
        /* ... */
    }
}
```

### Layouts

- [4.5.1](#4.5.1) <a name='4.5.1'></a> Components should be made in a way that they're reusable in different contexts. Avoid putting these properties in components:

    * Positioning (`position`, `top`, `left`, `right`, `bottom`)
    * Floats (`float`, `clear`)
    * Margins (`margin`)
    * Dimensions (`width`, `height`)

If you need to define these, try to define them in whatever context they will be in.

```sass
.article-list {
    & {
        @include clearfix;
    }

    > .article-card {
        width: 33.3%;
        float: left;
    }
}

.article-card {
    & {
        /* ... */
    }
    > .image {
        /* ... */
    }
    > .title {
        /* ... */
    }
    > .category {
        /* ... */
    }
}
```

### Conventions

- [5.1](#5.1) <a name='5.1'></a> Every colors need to be on the `colors.scss` file. The color name should have a `brand-` prefix for the brand primary colors and a tone as a suffix.

```sass
// bad
$green: green;
$green-2: green;

//good
$brand-green: green;
$brand-green-light: green;

```

- [5.2](#5.2) <a name='5.2'></a> For colors where the opacity changes a suffix must be added from the available list:
    * xs
    * s
    * m
    * l
    * -xl
    * -xxl
    * -xxxl
