#Workable CSS/Sass Styleguide

This is the coding style guide we use for writing CSS & Sass in Workable. 

A coding style guide is an essential tool to keep our code maintainable and scalable. Workable has a big codebase with lots of different aspects (core product, career pages, marketing website & resources) so we should try to make it easy for new developers and designers to dive into.

> Every line of code should appear to be written by a single person, no matter the number of contributors.

[Mark Otto](http://mdo.github.io/code-guide) 

Table of Contents | 
------------ | 
[Syntax & Formatting](https://github.com/Workable/css-style-guide#syntax--formatting) | 
[Sass](https://github.com/Workable/css-style-guide#sass) | 
[BEM](https://github.com/Workable/css-style-guide#bem) | 
[File Organization](https://github.com/Workable/css-style-guide#file-organization) | ι
[Recommended Reading](https://github.com/Workable/css-style-guide#recommended-reading) | 

***

## Syntax & Formatting

Consistent syntax and formatting is the easiest way we can ensure our codebase stays readable and clean. It's easier to keep writing clean code if the codebase is already well-structured and consistent.

The basic ideas are: 

* Use soft tabs with two spaces in your favourite text editor.
* Write multi-line CSS.
* Use dashes for your classes - no camelCase, no underscores unless using BEM.
* Use whitespace to make your code easy to read.
* Do not use ID selectors for styling.

### Multi-line CSS

Multi-line CSS is easier to scan, since your eye can keep tracking the left side of the screen instead of darting around. Moreover, it is also easier to work with in version control, diff merges and error reporting.

There are some exceptions to that rule, which you'll probably stumble upon anyway. For example, you can use single-line CSS when repeated rulesets have one declaration each:

```scss
h1 { font-size: $font-size-large * 2.75; }
h2 { font-size: $font-size-large * 2.25; }
h3 { font-size: $font-size-large * 1.75; }
h4 { font-size: $font-size-large * 1.25; }
h5 { font-size: $font-size-large; }
h6 { font-size: $font-size-large * 0.85; }
```

When using multiple selectors, every individual selector should have its own line.

```css
.selector,
.selector-secondary {
  ...
}
```


### Whitespace

You should add one space:

* Before the opening brace of declaration blocks for legibility.
* After `:` for each declaration.
* After each comma in comma-separated property values.
* After commas in `rgb()`, `rgba()`, `hsl()`, `hsla()`, or `rect()` values for legibility.

You should add one line:

* Between rulesets, to set them apart 
* Before nested selectors, to make parent selector styles differentiate 
* After file title comments (see below)

Here's an example:

```scss
.btn-small {
  font-size: 12px;
  padding: 6px 12px;
  margin-left: 5px;
  
  [class^="icon-w-"] {
    margin-right: 5px;
    position: relative;
    top: 1px;
  }
}

.btn-large {
  padding: 9px 14px;
  
  [class^="icon-w-"] {
    margin-right: 5px;
  }
}
```

This might seem excessive, but it really helps to keep the code easy to scan. When a file starts to be too long, it's time to refactor and break it down to smaller files. The code is minified on production anyway, so there's no reason not to use lots of whitespace.


### Ruleset syntax

* All hex values should be in lowercase for easier reference, e.g. `#ccc` instead of `#CCC`.
* Use shorthand hex values where applicable, e.g. `#369` instead of `#336699`.
* Use quotes for attribute values in selectors, e.g. `input[type="text"]` instead of `input[type=text]`.
* Avoid specifying units for zero values, e.g., `margin: 0` instead of `margin: 0px`.
* Your class names should be in lowercase. Use dashes to separate words, no underscores or camelCase, e.g. `.some-class` instead of `.someClass` or `.some_class`.

An example of a well written ruleset:

```scss
.selector,
.selector-secondary,
.selector[type="text"] {
  padding: 15px;
  margin: 0 0 15px;
  background-color: rgba(0, 0, 0, 0.5);
  box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;
}
```

### Comments

In large-scale projects, even a simple declarative language like CSS can cause problems when not documented. Add to this the inheritance and the way the cascade works and you'll be finding yourself in a difficult position if you don't comment your code efficiently.

Not everything needs to be commented, but some things really do. 

#### Add a title to your files

Every Sass file or partial has to be titled properly. A file title looks like this:

```scss
/*-----------------------------*\
 * ICONS
\*-----------------------------*/
```

#### Document your modules

Modules (or components) are reusable blocks of CSS code that follow a certain structure.  Each part of the module should be commented so that a developer coming after you can easily visualize the structure.

Add to each individual comment any modifier classes so that it's easy to see how the module interacts.

This is an example of a module comment:

```scss
/*
  Candidate module

  .candidate--selected       - Selected style when user clicks on a candidate
  .candidate--inactive       - Inactive style for processing candidates
*/
.candidate {
  ...
}
```

Don't forget to comment module elements as well: 

```scss
/*
  Checkbox on the left of candidate avatar
*/
.candidate__selector {
  ...
}
```
If you don't get the syntax/terminology above, don't worry - there's a whole section dedicated to BEM below.

Remember to use the syntax above to write a comment before a set of rules and not `//`. We use double slash only for inline comments (see below).

```
/*
   Use this type
*/
.candidate {
  ...
}

// Instead of this
.candidate {
  ...
}
```

#### Use inline comments for extras

Preprocessor (inline) comments can be used to specify the use of a specific declaration. They can be useful to denote todos or add quick tips for your future self. 

```scss
@import 'bootstrap/tooltip';
@import 'bootstrap/popovers';
@import 'bootstrap/utilities'; // keep last for overrides
```

Again, don't worry about going overboard with commenting, since comments and whitespace are removed during minification and aren't visible in production.


### Why not use IDs?

It might make sense to use an ID for a unique part of the page but believe me, you want to avoid it. Using IDs for styling increases the selector specificity immensely. If you want to override that styling at some point you have to use another ID (and probably nest it) or just slap `!important` on it and hope for the best. 

To demonstrate the ID problem with an example:

```html
<div id="footer">
	<div class="twitter">
		<a href="#">This is a link to my Twitter page</a>
	</div>
</div>
```

```scss
#footer { background-color: #000; }
#footer a { color: #fff; }

.twitter { background-color: #fff; }
.twitter a { color: #000; }
```

Let's say we have a black footer with white links, and we try to add a twitter widget with a white background and black links. The `#footer a { }` rule has much higher specificity than the `.twitter a { }` one, so now we end up with white links on a white background, unless we override it using `!important`.

This situation happens more than often in a fluid design system, so to keep things simple and as flat as possible, we're using classes everywhere.


### Naming conventions

As a rule of thumb, you should keep your class names as short as possible, without sacrificing clarity. For example, it's quite clear that `.btn` refers to a button, but avoid using classes like `.prfl`, `.usr` etc.

When working with a front-end developer, you should insist that no CSS classes should be used as Javascript hooks. Instead, you should add `.js-` prefixed classes to provide developers with a hook for elements. You shouldn't style those classes: they are to be used for Javascript purposes only. That way you don't have to edit Javascript file every time you change a class name - you only have to copy the `.js-` prefixed class. If you work with a kickass developer, they'll suggest using data attributes for this kind of stuff anyway.

Name things for people; they’re the only things that actually read your classes (everything else merely matches them). Once again, it is better to strive for reusable, recyclable classes rather than writing for specific use cases.

----------

## Sass

We use the SCSS flavour of Sass.

A typical order for Sass rules is:

* @extend at the top, so you can see right away which class or placeholder is extended
* @include after @extends
* regular styles next
* nested selectors last and nothing after them

Example:

```scss
.inline-form {
  @extend %attachments;
  @include gradient-vertical(#fafafa, #f2f2f2);
  border: 1px solid #ccc;
  padding: 10px;
  position: relative;
  margin-bottom: 25px;
    
  form {
    margin-bottom: 0;
    background: none;
    border: none;
  }
} 
```

Try to group related property declarations in four loose groups: **positioning**, **box model**, **typographic** and **visual**.
  
```scss
.declaration-order {
  /* Positioning */
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;

  /* Box-model */
  display: block;
  float: right;
  width: 100px;
  height: 100px;

  /* Typography */
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;

  /* Visual */
  background-color: #f5f5f5;
  border: 1px solid #e5e5e5;
  border-radius: 3px;

  /* Misc */
  opacity: 1;
}
```

Feel free to remove line breaks from single declarations if they refer to the same attribute, e.g. for sprite organization. It's easier to group them visually and edit them.

```scss
.sprite {
  display: inline-block;
  width: 16px;
  height: 15px;
  background-image: url(../img/sprite.png);
}

.icon           { background-position: 0 0; }
.icon-home      { background-position: 0 -20px; }
.icon-account   { background-position: 0 -40px; }
```

Avoid using shorthand notation unless you want to explicitly set all the available values at once. Setting all the values at once can lead to messy code full of unneeded overrides.

Check for excessive usage of these shorthands:

* margin
* padding
* font
* background
* border
* border-radius

Instead of this:

```scss
.element {
  margin: 0 0 10px;
  background: red;
  background: url("image.jpg");
  border-radius: 3px 3px 0 0;
}
```

Do this: 

```scss
.element {
  margin-bottom: 10px;
  background-color: red;
  background-image: url("image.jpg");
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
}
```

Keep your nesting to **3 levels maximum** to avoid specificity problems. Also, your nested rules shouldn't be more than 50 lines in height, any more than that and it has an impact on code legibility. Simplify your markup and refactor till that's possible.

When using BEM (see below), there's no need to nest modifier and elements in your Sass code. The maintenance cost will rise since the specificity will be hard to override. Use a flat structure instead.

Instead of this:

```scss
.box {
  border: 1px solid #ccc;
  
  &.box--medium { width: 75%; }
  &.box--large  { width: 90%; }
}
```
  
Do this:

```scss
.box         { border: 1px solid #ccc; }
.box--medium { width: 75%; }
.box--large  { width: 90%; }
```

Use descriptive comments to separate sections in your stylesheets. For modules, don't forget to add a simple example of how it's used and its modifier classes.

Example:

```scss
/*
  Main data row module. Usually combined with a .flag class, contains .flag__sections as cells
  When a data-row is expanded, the expanded content is contained in another data-row directly above the first one

  :hover                -  a light yellow background is shown on hover and actions cell is shown
  .data-row--expanded   -  light grey style, used when data row is expanded to show content
  .data-row__form       -  styling for data rows that contain forms
*/ 

.data-row {
  .
  .
}

.data-row--expanded {
  .
  .
}

.data-row__form {
  .
  .
}
```

### Selector intent

Selector Intent is the process of deciding and defining what you want to style and how you will go about selecting it. For example, if you are wanting to style your website’s main navigation menu, a selector like this would be incredibly unwise:

```scss
header ul {}
```

This selector’s intent is to style any ul inside any header element, whereas our intent was to style the site’s main navigation. This is poor Selector Intent: you can have any number of header elements on a page, and they in turn can house any number of uls, so a selector like this runs the risk of applying very specific styling to a very wide number of elements. This will result in having to write more CSS to undo the greedy nature of such a selector.

A better approach would be a selector like:

```scss
.site-nav {}
```

An unambiguous, explicit selector with good Selector Intent. We are explicitly selecting the right thing for exactly the right reason.

Poor Selector Intent is one of the biggest reasons for headaches on CSS projects. Writing rules that are far too greedy—and that apply very specific treatments via very far reaching selectors—causes unexpected side effects and leads to very tangled stylesheets, with selectors overstepping their intentions and impacting and interfering with otherwise unrelated rulesets.

<!--
* We use the HTML5 doctype in all our layouts. It's the easiest to memorize and ensures consistent rendering in all browsers.
* Avoid writing markup in Javascript files. Content gets harder to find, edit and test.
* Avoid setting too long class names for reusable modules, since BEM will bite you afterwards.
* Avoid using generic element tags in your CSS - use classes instead.
* Avoid extensive use of attribute selectors like `[class^="…"]` since they impact browser performance.
* Avoid using ids for styling. They naturally have greater so they're a nightmare to use when developing a flexible framework.-->

----------

## BEM

[BEM](http://bem.info) is a naming convention created by [Yandex](http://yandex.ru/). BEM - meaning block, element, modifier - is a smart way of naming your CSS classes for clarity and transparency. It might look a bit ugly at first, but it really shines when styling complex systems.

In Workable, we use a simplified BEM version that looks like this:

```scss
.block { }
.block__element { }
.block--modifier { }
```

* `.block` is your basic component (reusable module) 
* `.block__element` is a building block of `.block`
* `.block--modifier` contains styling for a different state of `.block`

A real life example in BEM:

```html
<div class="modal modal--dark">
	<h1 class="modal__header">This is a modal header</h1>
	<div class="modal__content">
		This is the modal content
	</div>
</div>
```

```scss
.modal { }
.modal--dark { }
.modal__header { }
.modal__content { }
```

* `.modal` is a basic modal window
* `.modal--dark` is a modifier on the basic modal style, a special case which requires its background to be dark, for example. Modifiers are not obligatory, they're only added when the design needs to differ than the default styling.
* `.modal__header` and `.modal__content` are child elements of the modal window and they can only exist when inside `.modal`


### Why BEM?
Because it permits you to keep the relationship between the building blocks of an element and its different states obvious. There is no ambiguity. 

Compare this:

```scss
.button.primary.left
```

...to this:

```scss
.button.button--primary.left
```

In the first case, the `.primary` class is ambiguous - what does it refer to? In the second case, you can clearly see that `.primary` is a modifier class of `.button` and it's probably used to tweak its colours.

This all seems pretty straightforward, but BEM shines in real-life use. Consider this code block:

```html
<ul class="list">
  <li>
    <h2 class="title left">John Doe</h2>
    <button class="btn small disabled right">Pending</button>
  </li>
</ul>
```

At a glance, you see a bunch of random classes that may be semantic but very abstract. For example, what's `.title`? How does `.small` affect `<button>`? You can't really see the relations between the classes.

Now compare it to its BEM equivalent:

```html
<ul class="list">
  <li>
    <h2 class="list__title left">John Doe</h2>
    <button class="btn btn--small btn--disabled right">Pending</button>
  </li>
</ul>
```
  
You can see at a glance that `.list__title` is a building block of `.list`, `.btn--small` and `.btn--disabled` are modifiers of the `.button` class while `.left` and `.right` are probably just generic utility classes that affect positioning.

### Ewww, ugly!
Some people are against BEM, claiming it makes code look ugly. We are not those people :) 

Its advantages far outweight the minor inconvenience of typing slightly longer class names. You shouldn't dismiss it based solely on how it looks.

A common question is why BEM uses double dashes and underscores for modifiers and elements. The answer is that single dashes can be used as a part of the class name, e.g.

```scss
.user-profile {}
.user-profile__avatar {}
.user-profile--expanded {}
```

### When not to use BEM

Everything is a reusable module, till something isn't. There are loads of cases when you don't need to BEM something, like for helper classes:

```scss
// Positioning classes
.left   { float: left; }
.right  { float: right; } 

// Fractional widths
.one-half   { width: 50%; }
.one-third  { width: 33%; }
.two-thirds { width: 66%; }
```

As you can see, this type of classes doesn't fall into any BEM category. The trick before using BEM is to ask "Are those objects related? Can any of them stand on its own?" 

For example, a logo could have a `.header__logo` class, but what if we wanted to show the logo on the footer, or on a sidebar? Adding a `.header__logo` class restricts us to only using it in the header.
  
Another BEM pitfall is very long class names. Since you can combine elements and modifiers, you can easily end up with code like this:

```html
<div class="block">
  <div class="block__header">
    <h1 class="block__header__title block__header__title--muted">John Doe</h1>
  </div>
</div>
```

There's no reason to chain elements and reflect the DOM in your class names. So instead of `.block__header__title`, you could just use `.block__title`. There's another benefit to that: what if you decide later to move the title outside the header? If you went with the first class name, you'd have to refactor your code. Keep your object relations loose.

Another tip to avoid abusing BEM is to use generic classes that can apply to multiple elements instead of modifiers. So there's no need to have both `.field--disabled` and `.form--disabled` modifier classes - you can go by with just using `.field.disabled` and `.form.disabled`. Other candidates for this are `.focused`, `.active` etc.

## File Organization

> We’re not designing pages, we’re designing systems of components.
> 
[Stephen Hay](http://bradfrostweb.com/blog/mobile/bdconf-stephen-hay-presents-responsive-design-workflow/)

The basic idea is this: **when you set out to style a new feature, you should have most of the scaffolding ready from previous features**. 

The bulk of the styles you create should be reusable. Everything is a class. It's better to use 3 to 4 classes that combine effects on a single element than create an elaborate but limited style block that won't be reused anywhere else. You have to think twice before adding a new module - check if you can subclass or tweak an existing module and achieve the same effect.

All CSS styles are automatically parsed into a single file, which is then served to the user. That means that you can go crazy with abstraction without worrying about multiple stylesheets and HTTP requests.

Style-wise, Workable is organized into major sections: 

* **Admin**, which contains styles for the admin interface we use internally
* **Application**, which contains the marketing website style
* **Authentication**, which applies to all the login/signup screens
* **Backend**, which is the actual app that users use
* **Frontend**, which contains styles & themes for the career pages
* **Offers**, which includes all marketing offer-specific styles

**Note** Our resources site & blog are hosted in [WPEngine](http://wpengine.com), even though they're styled to look exactly like the rest of our marketing site. Since they're a separate Wordpress mini-site, we don't use Sass for their styling. You can [check their themes out on Github](https://github.com/Workable/workable-resources).

Each major section has (or should have) its own manifest file. That's a single file, usually named `<section>.css.scss`, where import every partial you need. Manifest files should not contain any Sass code whatsoever. Their job is to bring together the different design elements. You can either include your Sass variables in the top of the manifest file or (recommended) create a separate `_variables.scss` partial and include it on top.

A typical file organization for a section is:

```
backend/
  base/
    _buttons.css.scss
    _forms.css.scss
    _typography.css.scss
    .
    .
    .
  modules/
    _boxes.css.scss
    _blank-slates.css.scss
    _dropdowns.css.scss
    _fileuploads.css.scss
    _filters.css.scss
    .
    .
    .
  sections/
    _billing.css.scss
    _job-editor.css.scss
    _candidate-browser.css.scss
    _account.css.scss
    .
    .
    .
backend.css.scss
```

### Sass variables

You can use Sass variables for any value that you find yourself repeating in your code. Some common examples are brand hex colours, font families, grid variables and magic numbers like typical border-radii and padding.

To group variables, you can use the reverse grouping naming convention, e.g. instead of `$primary-font-family` & $secondary-font-family, you can use $font-family-primary and $font-family-secondary. This makes visual grouping easier.

So instead of:

```scss
// Typography

$base-font-family: "Helvetica Neue", Helvetica, Arial, sans-serif !default;
$fancy-font-family: "Myriad W08", "Segoe UI", Calibri, sans-serif;

$alpha-font-size: 20px !default;
$beta-font-size: 18px !default;
$gamma-font-size: 16px !default;
$delta-font-size: 14px !default;
$epsilon-font-size: 13px !default;
$zeta-font-size: 12px !default;
```

Use:

```scss
// Typography

$font-family-base: "Helvetica Neue", Helvetica, Arial, sans-serif !default;
$font-family-fancy: "Myriad W08", "Segoe UI", Calibri, sans-serif;

$font-size-alpha: 20px !default;
$font-size-beta: 18px !default;
$font-size-gamma: 16px !default;
$font-size-delta: 14px !default;
$font-size-epsilon: 13px !default;
$font-size-zeta: 12px !default;
```

Avoid using overly abstract variable names like $blue, $red - more descriptive names like $color-blue, $color-red are better for legibility and maintenance, even if they take longer to type.

## Including third-party frameworks

If you use third-party frameworks like Compass, be sure to import only the parts you use in your design. No need to bloat the stylesheet with stuff you don't need.

So instead of:

```css
@import compass;
```
  
Use:

```css
@import "compass/reset";
@import "compass/utilities/general/clearfix";
@import "compass/css3/background-clip";
@import "compass/css3/box-sizing";
@import "compass/css3/images";
@import "compass/css3/opacity";
```

### Including partials

The manifest stylesheet is the place where all the partials used in the projects will be imported.

A partial is a Sass file that is not individually parsed, but meant to be included in other Sass files. Partials have filenames that start with an underscore. Even if our setup permits us to be lax in using the underscore naming convention, it's good to do it anyway to distinguish between normal stylesheets and modules.

Partials are divided into two categories: **modules** and **sections**. Modules are the partials that define the base styles used for forms, tables etc as well as reusable components like labels and buttons. Sections contain the section-specific styles that probably won't get reused anywhere, like billing-specific widgets.

### Shared styles

Sometimes, a particular module is meant to be reused across sections, like for example alerts or avatars. In that case, feel free to add the reusable module to the `shared/` subfolder in the `stylesheets/` base folder.

----------

## Recommended reading

* [CSS Guidelines by Harry Roberts](http://cssguidelin.es)
* [Code Guide by @mdo](http://mdo.github.io/code-guide)
* [Airbnb CSS/Sass styleguide](https://github.com/airbnb/css)
* [Sass Style Guide by Chris Coyier](http://css-tricks.com/sass-style-guide/)
* [My Current CSS and Sass Style Guide by Hugo Giraudel](http://www.sitepoint.com/css-sass-styleguide/)
* [WTF, HTML and CSS? by @mdo](http://wtfhtmlcss.com)
* [BEM 101 by Robin Rendle](https://css-tricks.com/bem-101/)
* [MindBEMding – getting your head ’round BEM syntax](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)
