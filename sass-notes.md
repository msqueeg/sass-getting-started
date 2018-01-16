#SASS: Syntactically awesome Stylesheets

## Agenda
* WHY Sass (or maybe better, why CSS)?
* Getting Started
  * Prerequisites
  * Installing Sass and Compass
  * Install CSS Parser
  * Set up a test project
* Sass features
* Dynamically change color
* Creating a Custom Grid
* Spriting with Sass and Compass
* Conclusions

## Why SASS (or maybe better, why CSS)?

Web development is complex because every page is comprised of three components written in 3 very different languages.

html, css, javascript

this complexity is, ironically, a result of the early attempt to make creating and editing web pages something that a non-technical person could do.

Anddressen quote on html

since html was so limited in comparison to what word processing equipment could do, css was added to the mix. But css was also deliberately limited to prevent it from scaring away non-technical users.

CSS quote

The problem is that, for professional work, we need that "extra rope" that CSS doesn't have.

one of the major trends in web development is to make it easier for programmers to use familiar languages to write javascript using preprocessor languages & transpilers.

For instance [Java to Javascript transpiling, coffeescript](https://github.com/jashkenas/coffeescript/wiki/list-of-languages-that-compile-to-js)

With CSS, especially there has been a trend towards building CSS preprocessors that extend CSS. There are a number of solutions out there, but I tend to recommend Sass for several reasons:

* Mature
* Feature Rich
* Large Community with wide approval
* Frameworks
	* [Compass](http://compass-style.org/)
	* [Bourbon](http://bourbon.io/)
	* [Susy](http://susy.oddbird.net/)
* CSS Compatible

A word on CSS Compatibility:
SASS has a bit of a history, which we don't need to go into here, but there are two syntaxes available: the original "indented syntax" and, a newer syntax called "Sassy CSS" (SCSS) that was made available from version 3 of SASS.

Sassy CSS is a superset of CSS 3, which means that __any valid CSS is also valid SCSS__. For me, Sassy CSS is a compelling argument for working developers. It allows you to incorporate the power of SCSS gradually, as you need it, without needing to re-learn how to do everything.

You can use Sass by itself or with a Framework. I've worked with Susy and Compass and I'm using Compass for this presentation mainly because it's the one I'm most familiar with. Compass is maintained by Chris Eppstein, one of the two core Sass designers, and it provides the following enhancements to the core Sass compiler:
* Vendor prefixing (based on the CanIUse database)
* Math helpers like cos, sin, pi
* color helpers like shade, tint
* image helpers like image-width and image-height
* an image sprite builder

***

## Getting Started

### Prerequisites
This tutorial assumes you already have a few tools:
* a text editor
* a relatively modern web browser such as Firefox, Chrome or MS Edge. (Having a web server could also be useful, but not strictly necessary)
* The [Ruby](https://www.ruby-lang.org/en/) programming language installed. 
Walking through installing Ruby is beyond the scope of this tutorial, but here are a few pointers:

* If you are running Windows, use the [Ruby Instaler](http://rubyinstaller.org/)
* For Linux, see the [Documentation Pages](https://www.ruby-lang.org/en/documentation/installation/ "Ruby installation documentation") to find more information for your specific distribution.
* For OSX/MacOS, the default Ruby install should be sufficient.

### Installing SASS & Compass

For today's presentation we will be using [SASS]() plus [Compass](http://compass-style.org/).

For Windows `gem install compass`

For Linux / OS X `sudo gem install compass`

### Install CSS Parser (optional)

### Set up a test project

 1. Create a test project: `compass create sass-test`
 2. Change Directory into the project `cd sass-test`
 3. run Compass `compass watch`

## Sass features

### variables
``` $font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
} ```

### Nesting
``` nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
} ```
### Partials

you can structure your css in separate files, and create re-usable snippets that can be included in other files. You can create a partial by underscoring the file name like so: `_partial.scss`. This lets Sass/Compass know that that file is a partial and will not compile it to css.


### Import

CSS has an import function that was intended to allow you to split your CSS into smaller more maintainable portions. However, this creates a separate HTTP request for every file. This slows down loading the page. Sass avoids this by combining the files you want to import and the file you're importing them into into one single file that gets served to the client.

`//_reset.scss`

``` @import 'reset';

body {
  font: 100% Helvetica, sans-serif;
  background-color: #efefef;
} ```

### Mixins

Keep your code DRY with Mixins (i.e., macros). Mixins are great for any time you have to repeat a block of code. For instance, vendor prefixing becomes much simpler:

```
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
     -moz-border-radius: $radius;
      -ms-border-radius: $radius;
          border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Vendor prefixing was once one of the #1 reasons why people used the Compass framework on top of Sass, because it automatically updated the prefix mixins based on the Can I Use database. Today, I'm not sure that is a compelling use case by itself

### Inheritance

using @extend lets you share a set of CSS properties from one selector to another. You can also create placeholder classes with `%` that will not print unless they are extended.

```
// This CSS won't print because %equal-heights is never extended.
%equal-heights {
  display: flex;
  flex-wrap: wrap;
}

// This CSS will print because %message-shared is extended.
%message-shared {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.message {
  @extend %message-shared;
}

.success {
  @extend %message-shared;
  border-color: green;
}

.error {
  @extend %message-shared;
  border-color: red;
}

.warning {
  @extend %message-shared;
  border-color: yellow;
}
```

### Operators

```
.container { width: 100%; }


article[role="main"] {
  float: left;
  width: 600px / 960px * 100%;
}

aside[role="complementary"] {
  float: right;
  width: 300px / 960px * 100%;
}

```

***

## Dynamically change text color based on a background color

```
<p class="notification notification-confirm">Confirmation</p>
<p class="notification notification-warning">Warning</p>
<p class="notification notification-alert">Alert</p>
```

```
@function set-notification-text-color($color) {
  @if (lightness($color) > 50) {
    @return #000000; // Lighter backgorund, return dark color
  } @else {
    @return #ffffff; // Darker background, return light color
  }
}
```

lightness is a [built in SASS function](http://sass-lang.com/documentation/Sass/Script/Functions.html#lightness-instance_method)

brightness function from [John W. Long](https://gist.github.com/jlong/f06f5843104ee10006fe)

***

## Simple Grid Mixins

#### making the column robust
Let's extend the column mixin to include a way of setting the width of the column. When it comes to column width, for me it's easiest to think in terms of fractions. To create three cells that are of equal width they each need to be 1/3 the width of the containing element (the row).

```
@mixin col($col, $sum, $align: top) {
  width: percentage($col/$sum);
  font-size: 16px;
  display: inline-block;
  vertical-align: $align;
}
```

Now let's extend this further to so that we can specify the gap between cells:

```
@mixin col($col, $sum, $gap: 1em, $align: top) {
  width: percentage($col/$sum);
  font-size: 16px;
  display: inline-block;
  vertical-align: $align;
  box-sizing: border-box;
  padding: 0 $gap;
}
```

To accommodate the first and last columns we can add two additional parameters:

```
@mixin col($col, $sum, $gap: 1em, $align: top, $first: false, $last: false) {
  width: percentage($col/$sum);
  font-size: 16px;
  display: inline-block;
  vertical-align: $align;
  box-sizing: border-box;
  padding-left: if($first, 0, $gap);
  padding-right: if($last, 0, $gap);
}
```
Now we can call `@include col(1, 3, $first: true)` for the first cell, or `@include col(1, 3, $last: true)` for the final cell and it will work correctly.

***
## Spriting with Sass and Compass

### the Sass Way
```
$icon-width: 24px;
$icon-height: 24px;
$icons: image-url('toolbar.png');

.media-icon {
  background-image: $icons;
  background-position: -($icon-width * 5) -($icon-height * 1);
  width: $icon-width;
  height: $icon-height;
}
```

### The Compass Way

The magic @import directive
```
@import "images/social/*.png";
```

```
@include all-social-sprites;
```
#### class names
If you want more control
```
@import "images/social/*.png";

.fb-icon { @include social-sprite(facebook); }
.rss-icon { @include social-sprite(rss); }
.twitter-icon { @include social-sprite(twitter); }
```

#### sprite maps
```
$icons: sprite-map("social/*.png");

.fb-icon { background: sprite($icons, facebook); }
.rss-icon { background: sprite($icons, rss); }
.twitter-icon { background: sprite($icons, twitter); }
```

[Offical Compass Spriting Tutorial](http://compass-style.org/help/tutorials/spriting/)

***
## Conclusions

### Why Sass?
CSS is an important component of web development, but it is critically lacking in important features. Languages like Sass, Less and Stylus add back in important features that make modern CSS developers more productive.

### Why not?

* It requires Ruby
* It can be slow, especially on large projects.
* Better ways of doing things?

### Future Directions
As I've alluded to earlier, Compass has evolved quite a bit in it's lifetime. And it will probably evolve further in the next few years. 

One of the big evolutions in Sass was the release of [Libsass](http://sass-lang.com/libsass), a C Library for Sass. This allowed for wrappers in a larger number of languages, removing the need for Ruby, and also made Sass much faster by replacing Ruby with compiled C code.

The release of Libsass has led to the development of Node.js / NPM modules for working with, and extending Sass. One project that has emerged that I'm keeping an eye on is Chris Eppstein's [Eyeglass extension manager for node-sass](https://github.com/sass-eyeglass/eyeglass).

### Next Steps:

Interested in using Sass in your next project? There are easier methods than what i've shown you tonight (but they cost money).

Two apps I recommend:

* [CodeKit](https://codekitapp.com/)
* [Prepos](https://prepros.io/)

Resources
* [The Sass Way](http://thesassway.com)

