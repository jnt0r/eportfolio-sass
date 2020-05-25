# Handout SASS
SASS is a Preprocessor for CSS with many features to boost your CSS development. In this handout I summarize the most important features of SASS.

## Setup
For using SASS you need to install it first. There are many ways to install it but I recommend using npm.
To install SASS using npm you simply run  `npm install -g sass` in your terminal. After the installation is completed you can verify that it’s working by typing `sass –-version` in your terminal. It should print `1.26.5 compiled with dart2js 2.7.2` or similar.

Go to https://sass-lang.com/install to choose a different option.

## Creating new project
When creating a new project you need to create a HTML file and at least one SASS file (.sass or .scss). You have to link the generating css file in your HTML file `<link rel="stylesheet" href="your-css.css">`. I recommend to put all SASS files and the generated css files into their own directory.

## Compiling/Watching your files

To start compiling your scss files you type `sass scss/main.scss:css/styles.css` into your terminal (change directory names and file names according to your project setup)

If you want to compile all SASS files inside a directory you can leave out the file names.

If you want to compile your files every time you save them simply add the `--watch` option to the command.

If you want to compress the output you can add the `--style compressed` option.

## .SASS vs. .SCSS

SASS contains two formatting conventions determined by the file extension. 

`.sass` is the original syntax that uses indention instead of curly braces and semicolons.

`.scss` is the newer and recommended syntax. It uses curly braces and semicolons and is completely compatible with CSS and any existing CSS libaries. Every CSS code is valid SCSS code.

SCSS is often also named SASS as it's the same tool and language and it only differs a little. All examples in this document and the presentation are in SCSS syntax.

## Nesting

In SASS you can nest your rules what results in shorter selectors and helps to mimic your HTML structure.

SCSS:
```
.content {
    h1 {
        ...
    }
    p {
        ...
    }
    a {
        ...
    }
}
a {
    ...
}
```

CSS:

```
.content h1 {
    ...
}
.content p {
    ...
}
.content a {
    ...
}
a {
    ...
}
```

## Parent Selector

The Parent Selector (&) can be used in nested selectors to refer to the outer selector.

SCSS:
```
.content {
    color: blue;

    &:hover {
        color: red;
    }
}
```

CSS:
```
.content {
    color: blue;
}
.content:hover {
    color: red;
}
```

## Variables

You can define variables and use them as values. Variables can be declared everywhere but must start with a `$`. As you can see in the example below variables have scopes.

```
$global-var: global value;

.a {
    $local-var: local value;
    global: $global-var;
    local: $local-var;
}

.b {
    global: $global-var;
    // local: $local-var; $local-var is out of scope. Only accessible inside .a
}
```

CSS:
```
.a {
    global: global value;
    local: local value;
}

.b {
    global: global value;
}
```

Variables can have following types:

- Boolean
- Number
- Color
- String
- List (e.g. `(10, 20, 30)`)
- Map (e.g. `(1: 10, 2: 20, 3: 30)`)
- Null

## Interpolation

Interpolation embeds the result of an expression into the CSS. As an example, an expression can be an variable or an mathematical calculation.

With interpolation you can generate variable selectors, attribute names and attribute values.

Interpolation is used by putting the expression inside `#{expression}`. For example:

- `#{$my-variable}`

- `#{ 10 + 20 * 4 }`

## Partials

Partials are SASS files that are only imported in other files. They help to modularize your code and make it easier to maintain.

Partials have a underscore as prefix (e.g. `_partial.scss`). They will be embedded in the importing file and not printed into an own file. The Variables, Mixins, Functions and other SASS stuff get imported too.

Partials are imported by `@import 'partial-name'`. You don't need to write the underscore and the file extension. You have to specify the directory if the partial is located in a subdirectory.

## Mixins

Mixins are reusable code that can take parameters.

A mixin is defined with `@mixin mixin-name(parameters) {...}` and can be used with `@include mixin-name`.

SCSS
```
@mixin square($size, $radius: 0) {
  width: $size;
  height: $size;
  border-radius: $radius;
}

.avatar {
  @include square(100px);
}

.avatar-round {
  @include square(100px, 10px);
  border-color: black;
}
```

CSS

```
.avatar {
  width: 100px;
  height: 100px;
  border-radius: 0;
}

.avatar-round {
  width: 100px;
  height: 100px;
  border-radius: 10px;
  border-color: black;
}
```

## Inheritance

Inheritance allows to inherit the styles of another selector. With `@extend selector` the current selector is added to all rule-blocks that apply to the `selector`. Instead of copying the code (lik mixins would) inheritance updates existing selectors.

Placeholder Selectors (%) can be extended but are not present in the CSS.

SCSS
```
.foreground {
  color: red;
  
  &:hover {
    color: blue;
  }
}

%background {
  background: green;
}

.c {
  @extend .foreground;
  @extend %background;
  width: 100%;
}
```

CSS
```
.foreground, .c {
  color: red;
}
.foreground:hover, .c:hover {
  color: blue;
}

.c {
  background: green;
}

.c {
  width: 100%;
}
```

## Flow Control

In SASS you have the possibility to control whether styles get emitted, or to emit them multiple times with slight variations. This can be used inside or outside of selectors and to define styles or to change variables.

Following options are possible:

- If / Else If / Else (`@if`, `@else if`, `@else`)
- For Each (`@each`)
- For Range (`@for`)
- While (`@while`)

## Built-in function

SASS comes with several built-in functions for different use cases. These functions can be used in attribute values, interpolation, variables, mixins and other SASS features.

SASS provides functions to work with:

- Colors (lighten, darken, …)
- Lists (append, index, …)
- Maps (has-key, merge, …)
- Math (floor, abs, …)
- Meta (variable-exists, …)
- Selectors (extend, replace, …)
- String (slice, to-upper-case, …)

## Further Reading
Take a look at the SASS Documentation: https://sass-lang.com/documentation.
