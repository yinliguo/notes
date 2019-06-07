## Sass
> .scss扩展名文件，是css超集，括号嵌套的形式；.sass扩展名文件，没有花括号和分号，使用缩进区分

### 变量
```
// scss
$base-color: #c6538c;
$border-dark: rgba($base-color, 0.88);

.alert {
  border: 1px solid $border-dark;
}

// css
.alert {
  border: 1px solid rgba(198, 83, 140, 0.88);
}
```
* 默认值，可以被覆盖，但要在上面

```
// _library.scss
$black: #000 !default;
$border-radius: 0.25rem !default;
$box-shadow: 0 0.5rem 1rem rgba($black, 0.15) !default;

code {
  border-radius: $border-radius;
  box-shadow: $box-shadow;
}

// style.scss
$black: #222;
$border-radius: 0.1rem;

@import 'library';

// css
code {
  border-radius: 0.1rem;
  box-shadow: 0 0.5rem 1rem rgba(#222, 0.15);
}
```
* 作用域为花括号

```
// scss
$variable: global value;

.content {
  $variable: local value;
  value: $variable;
}

.sidebar {
  value: $variable;
}

// css
.content {
  value: local value;
}

.sidebar {
  value: global value;
}
```
* 在流程控制中的变量只会给前面的变量进行赋值，而不是覆盖

```
// scss
$dark-theme: true !default;
$primary-color: #f8bbd0 !default;
$accent-color: #6a1b9a !default;

@if $dark-theme {
  $primary-color: darken($primary-color, 60%);
  $accent-color: lighten($accent-color, 60%);
}

.button {
  background-color: $primary-color;
  border: 1px solid $accent-color;
  border-radius: 3px;
}

// css
.button {
  background-color: #750c30;
  border: 1px solid #f5ebfc;
  border-radius: 3px;
}
```
* 变量类型

```
// *** Number ***
@debug 100; // 100
@debug 0.8; // 0.8
@debug 16px; // 16px
@debug 5px * 2px; // 10px*px (read "square pixels")
@debug 5.2e3; // 5200
@debug 6e-2; // 0.06
@debug 0.012345678912345; // 0.0123456789
@debug 0.01234567891 == 0.01234567899; // true
@debug 1.00000000009; // 1
@debug 0.99999999991; // 1

// *** String ***
@debug unquote(".widget:hover"); // .widget:hover
@debug quote(bold); // "bold"

@debug "\""; // '"'
@debug \.widget; // \.widget
@debug "\a"; // "\a" (a string containing only a newline)
@debug "line1\a line2"; // "line1\a line2"
@debug "Nat + Liz \1F46D"; // "Nat + Liz 👭"

@debug "Helvetica Neue"; // "Helvetica Neue"
@debug "C:\\Program Files"; // "C:\\Program Files"
@debug "\"Don't Fear the Reaper\""; // "\"Don't Fear the Reaper\""
@debug "line1\a line2"; // "line1\a line2"

$roboto-variant: "Mono";
@debug "Roboto #{$roboto-variant}"; // "Roboto Mono"

@debug bold; // bold
@debug -webkit-flex; // -webkit-flex
@debug --123; // --123

$prefix: ms;
@debug -#{$prefix}-flex; // -ms-flex

// *** Colors ***
@debug #f2ece4; // #f2ece4
@debug #b37399aa; // rgba(179, 115, 153, 67%)
@debug midnightblue; // #191970
@debug rgb(204, 102, 153); // #c69
@debug rgba(107, 113, 127, 0.8); // rgba(107, 113, 127, 0.8)
@debug hsl(228, 7%, 86%); // #dadbdf
@debug hsla(20, 20%, 85%, 0.7); // rgb(225, 215, 210, 0.7)

// *** Bool ***
@debug 1px == 2px; // false
@debug 1px == 1px; // true
@debug 10px < 3px; // false
@debug comparable(100px, 3in); // true

@debug true and true; // true
@debug true and false; // false

@debug true or false; // true
@debug false or false; // false

@debug not true; // false
@debug not false; // true

@debug if(true, 10px, 30px); // 10px
@debug if(false, 10px, 30px); // 30px

// null
@debug str-index("Helvetica Neue", "Roboto"); // null
@debug map-get(("large": 20px), "small"); // null
@debug &; // null

$fonts: ("serif": "Helvetica Neue", "monospace": "Consolas");

h3 {
  font: 18px bold map-get($fonts, "sans");// font: 18px bold;
}

// *** List ***
(Helvetica, Arial, sans-serif)
(10px 15px 0 0)
[line1 line2]

@debug nth(10px 12px 16px, 2); // 12px
@debug nth([line1, line2, line3], -1); // line3

@debug append(10px 12px 16px, 25px); // 10px 12px 16px 25px
@debug append([col1-line1], col1-line2); // [col1-line1, col1-line2]

@debug index(1px solid red, 1px); // 1
@debug index(1px solid red, solid); // 2
@debug index(1px solid red, dashed); // null

$sizes: 40px, 50px, 80px;

@each $size in $sizes {
  .icon-#{$size} {
    font-size: $size;
    height: $size;
    width: $size;
  }
}

// *** Map ***
 (<expression>: <expression>, <expression>: <expression>)

$font-weights: ("regular": 400, "medium": 500, "bold": 700);

@debug map-get($font-weights, "medium"); // 500
@debug map-get($font-weights, "extra-bold"); // null

$icons: ("eye": "\f112", "start": "\f12e", "stop": "\f12f");

@each $name, $glyph in $icons {
  .icon-#{$name}:before {
    display: inline-block;
    font-family: "Icon Font";
    content: $glyph;
  }
}

$light-weights: ("lightest": 100, "light": 300);
$heavy-weights: ("medium": 500, "bold": 700);

@debug map-merge($light-weights, $heavy-weights);

// *** Function ***
@function remove-where($list, $condition) {
  $new-list: ();
  $separator: list-separator($list);
  @each $element in $list {
    @if not call($condition, $element) {
      $new-list: append($new-list, $element, $separator: $separator);
    }
  }
  @return $new-list;
}

$fonts: Tahoma, Geneva, "Helvetica Neue", Helvetica, Arial, sans-serif;

content {
  @function contains-helvetica($string) {
    @return str-index($string, "Helvetica");
  }
  font-family: remove-where($fonts, get-function("contains-helvetica"));
}
```

### 表达式
#### 运算符
```
==, !=, +, -, *, /, %, <, <=, >, >=, and, or, not, 
```

### 语法
#### 父选择器
```
// scss
.alert {
  &:hover {
    font-weight: bold;
  }

  [dir=rtl] & {
    margin-left: 0;
    margin-right: 10px;
  }

  :not(&) {
    opacity: 0.8;
  }
}

// css
.alert:hover {
  font-weight: bold;
}

[dir=rtl] .alert {
  margin-left: 0;
  margin-right: 10px;
}

:not(.alert) {
  opacity: 0.8;
}
```
#### 占位选择器
```
// scss
%toolbelt {
  box-sizing: border-box;
  border-top: 1px rgba(#000, .12) solid;
  padding: 16px 0;
  width: 100%;

  &:hover { border: 2px rgba(#000, .5) solid; }
}

.action-buttons {
  @extend %toolbelt;
  color: #4285f4;
}

.reset-buttons {
  @extend %toolbelt;
  color: #cddc39;
}

// css
.action-buttons, .reset-buttons {
  box-sizing: border-box;
  border-top: 1px rgba(0, 0, 0, 0.12) solid;
  padding: 16px 0;
  width: 100%;
}
.action-buttons:hover, .reset-buttons:hover {
  border: 2px rgba(0, 0, 0, 0.5) solid;
}

.action-buttons {
  color: #4285f4;
}

.reset-buttons {
  color: #cddc39;
}
```
#### 嵌套
```
// scss	
nav {
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
}

// css
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
nav li {
  display: inline-block;
}
nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```
```
// scss
.alert, .warning {
  ul, p {
    margin-right: 0;
    margin-left: 0;
    padding-bottom: 0;
  }
}

// css
.alert ul, .alert p, .warning ul, .warning p {
  margin-right: 0;
  margin-left: 0;
  padding-bottom: 0;
}
```
```
// scss
ul > {
  li {
    list-style-type: none;
  }
}

// css
ul > {
  li {
    list-style-type: none;
  }
}
```
```
// scss
.info-page {
  margin: auto {
    bottom: 10px;
    top: 2px;
  }
}

// css
.info-page {
  margin: auto;
  margin-bottom: 10px;
  margin-top: 2px;
}
```
#### mixin（插值）
```
// scss
@mixin define-emoji($name, $glyph) {
  span.emoji-#{$name} {
    font-family: IconFont;
    font-variant: normal;
    font-weight: normal;
    content: $glyph;
  }
}

@include define-emoji("women-holding-hands", "👭");

// css
@charset "UTF-8";
span.emoji-women-holding-hands {
  font-family: IconFont;
  font-variant: normal;
  font-weight: normal;
  content: "👭";
}
```
```
// scss
@mixin prefix($property, $value, $prefixes) {
  @each $prefix in $prefixes {
    -#{$prefix}-#{$property}: $value;
  }
  #{$property}: $value;
}

.gray {
  @include prefix(filter, grayscale(50%), moz webkit);
}

// css
.gray {
  -moz-filter: grayscale(50%);
  -webkit-filter: grayscale(50%);
  filter: grayscale(50%);
}
```
#### At-Rules
##### @import
* 下划线开头的文件不会单独编译

```
// scss
// _theme.scss
pre, code {
  font-family: 'Source Code Pro', Helvetica, Arial;
  border-radius: 4px;
}

// style.scss
.theme-sample {
  @import "theme";
}

// css
.theme-sample pre, .theme-sample code {
  font-family: 'Source Code Pro', Helvetica, Arial;
  border-radius: 4px;
}
```
##### @mixin & @include
* 可选参数

```
@mixin replace-text($image, $x: 50%, $y: 50%) {...}
```
* 不定参数

```
// scss
@mixin order($height, $selectors...) {
  @for $i from 0 to length($selectors) {
    #{nth($selectors, $i + 1)} {
      position: absolute;
      height: $height;
      margin-top: $i * $height;
    }
  }
}

@include order(150px, "input.name", "input.address", "input.zip");

// css
input.name {
  position: absolute;
  height: 150px;
  margin-top: 0px;
}

input.address {
  position: absolute;
  height: 150px;
  margin-top: 150px;
}

input.zip {
  position: absolute;
  height: 150px;
  margin-top: 300px;
}
```
* 内容块

```
// scss
@mixin hover {
  &:not([disabled]):hover {
    @content;
  }
}

.button {
  border: 1px solid black;
  @include hover {
    border-width: 2px;
  }
}

// css
.button {
  border: 1px solid black;
}
.button:not([disabled]):hover {
  border-width: 2px;
}
```
##### @function
```
// scss
@function invert($color, $amount: 100%) {
  $inverse: change-color($color, $hue: hue($color) + 180);
  @return mix($inverse, $color, $amount);
}

$primary-color: #036;
.header {
  background-color: invert($primary-color, 80%);
}

// css
.header {
  background-color: #523314;
}
```
##### @extend
```
// scss
.error {
  border: 1px #f00;
  background-color: #fdd;

  &--serious {
    @extend .error;
    border-width: 3px;
  }
}

// css
.error, .error--serious {
  border: 1px #f00;
  background-color: #fdd;
}
.error--serious {
  border-width: 3px;
}
```
```
// scss
.error:hover {
  background-color: #fee;
}

.error--serious {
  @extend .error;
  border-width: 3px;
}

// css
.error:hover, .error--serious:hover {
  background-color: #fee;
}

.error--serious {
  border-width: 3px;
}
```
##### @error, @warn, @debug
##### @at-root
```
// scss
@mixin unify-parent($child) {
  @at-root #{selector-unify(&, $child)} {
    @content;
  }
}

.wrapper .field {
  @include unify-parent("input") {
    /* ... */
  }
  @include unify-parent("select") {
    /* ... */
  }
}

// css
.wrapper input.field {
  /* ... */
}
.wrapper select.field {
  /* ... */
}
```
```
// scss
@media print {
  .page {
    width: 8in;

    @at-root (without: media) {
      color: #111;
    }

    @at-root (with: rule) {
      font-size: 1.2em;
    }
  }
}

// css
@media print {
  .page {
    width: 8in;
  }
}
.page {
  color: #111;
}
.page {
  font-size: 1.2em;
}
```
##### @if, @else, @else if
```
// scss
@mixin triangle($size, $color, $direction) {
  height: 0;
  width: 0;

  border-color: transparent;
  border-style: solid;
  border-width: $size / 2;

  @if $direction == up {
    border-bottom-color: $color;
  } @else if $direction == right {
    border-left-color: $color;
  } @else if $direction == down {
    border-top-color: $color;
  } @else if $direction == left {
    border-right-color: $color;
  } @else {
    @error "Unknown direction #{$direction}.";
  }
}

.next {
  @include triangle(5px, black, right);
}

// css
.next {
  height: 0;
  width: 0;
  border-color: transparent;
  border-style: solid;
  border-width: 2.5px;
  border-left-color: black;
}
```
##### @each
* `@each <variable> in <expression> { ... }`

```
$sizes: 40px, 50px, 80px;

@each $size in $sizes {
  .icon-#{$size} {
    font-size: $size;
    height: $size;
    width: $size;
  }
}
```
* `@each <variable>, <variable> in <expression> { ... }`

```
$icons: ("eye": "\f112", "start": "\f12e", "stop": "\f12f");

@each $name, $glyph in $icons {
  .icon-#{$name}:before {
    display: inline-block;
    font-family: "Icon Font";
    content: $glyph;
  }
}
```
* `@each <variable...> in <expression> { ... }`

```
$icons:
  "eye" "\f112" 12px,
  "start" "\f12e" 16px,
  "stop" "\f12f" 10px;

@each $name, $glyph, $size in $icons {
  .icon-#{$name}:before {
    display: inline-block;
    font-family: "Icon Font";
    content: $glyph;
    font-size: $size;
  }
}
```
##### @for
* `@for <variable> from <expression> to <expression> { ... }`不包含最后一个值
* `@for <variable> from <expression> through <expression> { ... }`包含最后一个值

```
// scss
$base-color: #036;

@for $i from 1 through 3 {
  ul:nth-child(3n + #{$i}) {
    background-color: lighten($base-color, $i * 5%);
  }
}

// css
ul:nth-child(3n + 1) {
  background-color: #004080;
}

ul:nth-child(3n + 2) {
  background-color: #004d99;
}

ul:nth-child(3n + 3) {
  background-color: #0059b3;
}
```
##### @while
```
// scss
/// Divides `$value` by `$ratio` until it's below `$base`.
@function scale-below($value, $base, $ratio: 1.618) {
  @while $value > $base {
    $value: $value / $ratio;
  }
  @return $value;
}

$normal-font-size: 16px;
sup {
  font-size: scale-below(20px, 16px);
}

// css
sup {
  font-size: 12.36094px;
}
```
#### 内置函数
##### css原生函数
```
:root {
  --primary-color: red;
  --logo-text: var(--primary-color);
}
```
##### Number函数
```
abs($number)
ceil($number)
floor($number)
round($number)
comparable($number1, $number2)
max($number...)
min($number...)
percentage($number)
random($limit: null)
unit($number)
unitless($number)
```
##### String函数
```
quote($string)
unquote()
str-index($string, $substring)
str-insert($string, $insert, $index)
str-length($string)
str-slice($string, $start-at, $end-at: -1)
to-upper-case($string)
to-lower-case($string)
unique-id()
```
##### Color函数
```
adjust-color($color, $red: null, $green: null, $blue: null, $hue: null, $saturation: null, $lightness: null, $alpha: null)
change-color($color, $red: null, $green: null, $blue: null, $hue: null, $saturation: null, $lightness: null, $alpha: null)
adjust-hue($color, $degrees)
complement($color)
darken($color, $amount)
desaturate($color, $amount)
grayscale($color)
opacity($color)
blue($color)
green($color)
red($color)
hsl($hue $saturation $lightness)
hue($color)
ie-hex-str($color)
invert($color, $weight: 100%)
lighten($color, $amount)
lightness($color)
mix($color1, $color2, $weight: 50%)
opacify($color, $amount)
fade-in($color, $amount)
rgb($red $green $blue)
saturate($color, $amount)
saturation($color)
scale-color($color, $red: null, $green: null, $blue: null, $saturation: null, $lightness: null, $alpha: null)
transparentize($color, $amount)
fade-out($color, $amount)
```
##### List函数
```
append($list, $val, $separator: auto)
index($list, $value)
is-bracketed($list)
join($list1, $list2, $separator: auto, $bracketed: auto)
length($list)
list-separator($list)
nth($list, $n) 
set-nth($list, $n, $value)
zip($lists...)
```
##### Map函数
```
keywords($args)
map-get($map, $key) 
map-has-key($map, $key)
map-keys($map)
map-merge($map1, $map2)
map-remove($map, $keys...)
map-values($map)
```
##### 选择器函数
```
is-superselector($super, $sub)
selector-append($selectors...)
selector-extend($selector, $extendee, $extender)
selector-nest($selectors...)
selector-parse($selector)
selector-replace($selector, $original, $replacement)
selector-unify($selector1, $selector2)
simple-selectors($selector)
```
##### 检查函数
```
call($function, $args...) 
content-exists()
feature-exists($feature)
function-exists($name)
get-function($name, $css: false)
global-variable-exists($name)
inspect($value)
mixin-exists($name)
type-of($value)
variable-exists($name)
```