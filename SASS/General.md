[Video used](https://youtu.be/BEdCOvJ5RY4)
File naming:  SCSS or SASS.  SCSS looks like normal CSS, SASS looks unfamiliar.
Gets compiled to /dist/*.css (unless you use a bundler like Parcel)

### Partials
Allow you to store CSS in smaller chunks, by the type of code it is, for example putting all the buttons styles together.  File names should start with an underscore.  To bring in, use either `@import` or `@use` - older compilers might not support `@use`.  Syntax to import is `@import './filename';` - not there is no leading _ or file extension.

`@import` always shares everything globally (variables etc), also used by bootstrap, probably a better choice

`@use` doesn't share variables well, it requires the namespace to be entered before variables (or `@use './filename' as *;`)

Organizing can be done with the [SASS 7-1 pattern](https://sass-guidelin.es/#the-7-1-pattern) so each partial is in another folder and the `main.scss` just references all of them.

### Variables
\- and _ are treated the same.  Prefix with a $
They can hold all kinds of values (numbers, hex codes, etc), arrays (separated by a space or comma), and value maps like this:
```
// Variable declarations
$red: #FF595E;
$blue: #0A2463;
$yellow: #F9CB40;

// Variable array declaration:
$cursors: pointer auto inherit;

// SASS Map
$theme-colors: (
    "primary": $red,
    "secondary": $blue,
    "warning": $yellow
);  // don't forget the trailing ; here
```

Cycle through maps with this syntax:
```
@each $color, $value in $theme-colors {
  .btn-#{$color} {    // this is how to interpolate the string.
    background-color: $value;
    // all other theme styles here
  }
}
```
Or if it's just a list:
```
$cursors: pointer auto inherit;

@each $cursor in $cursors {
    .cursor-#{$cursor} {   // this is how to interpolate the string.
        cursor: $cursor;
    }
}
```

The above is similar to how Bootstrap works, and would allow you to add a class of btn-primary etc to an HTML element for styling.

### Mixins

Gives a way to duplicate reusable CSS chunks, kept in the `abstracts` folder.  Can take arguments.  Usage:

**./abstracts/mixins.scss**
```
@mixin button($value) {
  background-color: $value;
  color: white !important;
  border-radius: 4px;
  transition: all ease-in .2s;
  }
```
**./components/buttons**
```
@use '../abstracts/variables' as *; 
@use '../abstracts/mixins' as *;

@each $color, $value in $theme-colors {
  .btn-#{$color} {
    @include button($value);
  }
}
```

### Functions
Similar to a mixin, but it returns a single value rather than a collection of lines.  Can also take in lines

```
@function lighten-color($color) {
  $complement: complement($color);  // built in SASS variable to get a complementing color
  $light-complement: lighten($complement);
  @return $light-complement;
}