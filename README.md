# GML Coding Style
> Coding style used in all ♥x1 GameMaker projects

# Table of Contents
* [General overview](#general-overview)
* [Project naming](#project-naming)
* [Resource naming](#resource-naming)
    * [Scripts](#scripts)
    * [Other resources](#other-resources)
    * [Library prefix](#library-prefix)
    * [Special cases](#special-cases)
* [Project structure](#project-structure)
* [Whitespace](#whitespace)
    * [Horizontal](#horizontal)
    * [Vertical](#vertical)
* [Line length](#line-length)
    * [Wrapping](#wrapping)
* [Variables](#variables)
* [Functions](#functions)
  * [Anonymous functions](#anonymous-functions)
  * [Constructors](#constructors)
* [Structs](#structs)
* [Enums](#enums)
* [Macros](#macros)
* [If statements](#if-statements)
* [Switch](#switch)
* [Pragma](#pragma)

# General overview
* Slagtand follows C-like coding conventions for GML.
* Semicolons and parentheses (in if statements etc.) are mandatory.
* Use Allman indentation style (braces are on separate lines, with the same
indentation level as the statement which they follow). Only exception to this are
anonymous functions and structs, which should have the opening brace on the same line.
* For indentation use tabs only! Configure your IDE to use 4 characters wide tabs.
* Braces are mandatory, even if they surround only one statement.
* Library prefixes are used to differentiate between library and project specific
resources/scripts/enums/variables.
* All resources (except for scripts) and enums have `UpperCamelCase` names.
* Scripts and variables are `lowercase_separated_by_underscores`.
* Macros are `CAPITALIZED_WITH_UNDERSCORES`.
* Use of keyword alternatives to `{`, `&&`, `||`, etc. like `begin`, `and`, `or`,
etc. is forbidden.
* Soft max for line length is 80 characters, hard max is 100.
* Use [GMDoc](https://github.com/kraifpatrik/gmdoc) to document your code.

# Project naming
All *.yyp project names are lowercase, with words separated by `-` (minus sign).
If a project is a sub-project of a larger library, it should be prefixed with a
prefix common for the library. Prefixes are lowercase abbreviations.

```
Library name: CE
Common prefix: ce
Project names: ce-assert, ce-string-utils, ce-tween
```

# Resource naming
## Scripts
All script names are `lowercase_separated_by_underscores` and they don't have a
type prefix: `load_level`, `render_static_object`, `send_data`, ...

## Other resources
All other resources names are `UpperCamelCase`, prefixed by the resource type:

Resource | Prefix | Example
-------- | ------ | -------
Sprite   | `Spr`  | `SprButton`, `SprPlayer`
Tile Set | `Ts`   | `TsCity`, `TsJungle`
Sound    | `Snd`  | `SndClick`, `SndExplosion`
Path     | `Pth`  | `PthDunnoIDontUseThese`
Shader   | `Sh`   | `ShBlur`, `ShColorGrading`
Font     | `Fnt`  | `FntDefault`, `FntTitle`
Timeline | `Tml`  | `TmlIDontUseTheseEither`
Object   | `O`    | `OBullet`, `OController`
Room     | `Rm`   | `RmMenu`, `RmLoadingScreen`

## Library prefix
All resources which are part of a library should be prefixed by the common
library prefix followed by `_` (underscore)! Whether the library prefix is
lowercase or uppercase is determined by the resource type. `UpperCamelCase`
named resources have *uppercase* library prefix,
`lowercase_separated_by_underscores` named resources have *lowercase* library
prefix.

```
Library name: CE
Common prefix: ce
Resource names: CE_SprButton, CE_OController, CE_RmEditor
Script names: ce_load_level, ce_render_static_object, ce_send_data
```

If a resource is only used for example/demo/testing purposes, it shouldn't be
prefixed by a common library prefix.

## Special cases
If you have a collection of scripts that operate only on a specific type of
object, they should be prefixed by the object name (without the object prefix):

```
Object: OPlayer
Scripts: player_get_health, player_set_experience, player_spawn

Object: CE_OButton
Scripts: ce_button_create, ce_button_draw, ce_button_on_click
```

# Project structure
* All folder names are `UpperCamelCase`.
* All project resources that should be included upon `catalyst install` must be
put into a folder named after the project.
* If a project contains resources for example/demo/testing/etc. purposes only,
they must be put into a separate folder called like `Example`/`Demo`/`Test`/etc.
These folders must be ignored in the `catalyst.json` file.
* All folders and resources are ordered lexicographically. Do not mix folders
between resources - folders go first, resources follow after the last folder!

**Example:**

Resource tree:
```
Scripts/
|-- DrawUtilities/
|   |-- Text/
|   |   |-- ce_draw_text_bold
|   |   |-- ce_draw_text_italic
|   |   `-- ce_draw_title
|   |-- ce_draw_circle
|   `-- ce_draw_rectangle
`-- Tests/
    `-- draw_menu_button
```

catalyst.json:
```json
{
    "name": "slagtand-org/ce-draw-utilities",
    "ignore": {
        "Tests": "group"
    }
}
```

# Whitespace
## Horizontal
* Make sure that your IDE is set to use tabs with width of 4 characters. Indent
code using tabs only!

* Always put a single space after comma.

```gml
// Bad
draw_text_color(x,y,"Hello World!",c_aqua,c_aqua,c_blue,c_blue,1);

// Good
draw_text_color(x, y, "Hello World!", c_aqua, c_aqua, c_blue, c_blue, 1);
```

* Do not put whitespace around parentheses or brackets.

```gml
// Bad
do_something ( grid[ # x, y ] );

// Good
do_something(grid[# x, y]);
```

* Always surround operators with a single space.

```gml
// Bad
x+=23*((2+4)/sin(y));

// Good
x += 23 * ((2 + 4) / sin(y));
```

* If you wish to form columns from multiple consecutive assignments, you can use
spaces, though this is generally discouraged, since it can become very tedious
to maintain.

```gml
// Allowed, but discouraged
direction   = random(360);
health      = 100;
image_speed = 1;

enum EColors
{
    Red   = $0000FF,
    Green = $00FF00,
    Blue  = $FF0000
};

// Good
direction = random(360);
health = 100;
image_speed = 1;

enum EColors
{
    Red = $0000FF,
    Green = $00FF00,
    Blue = $FF0000
};
```

* Lines shouldn't contain trailing whitespace.

## Vertical
* Use at most one empty line to separate blocks of code.

* It is a good practice to put an empty line before and after control flow
blocks to improve code readability, though not necessary for few-line scripts.

```gml
var _x = 0;
var _y = 0;
var _line_height = string_height("Q");
var _message_count = ds_list_size(messages);

for (var i = 0; i < _message_count; ++i)
{
    draw_text(_x, _y, messages[| i]);
    _y += _line_height;
}

// Continue here, after a blank line...
```

* Do not put empty line at end of files.

# Line length
Code grows mainly vertically, not horizontally. You should always target 80
characters at max, but there is an allowed reserve of 20 characters (so 100 at
most) in case that line wrapping would result in weird indentation. Projects or
pull requests with lines with much over the 100 characters limit won't be
accepted!

## Wrapping
Wrap long lines with new line and one level deeper indentation.

```gml
// Bad
draw_rectangle_color(x, y, x + width, y + height, image_blend, image_blend,
                     image_blend, image_blend, false);

// Good
draw_rectangle_color(x, y, x + width, y + height, image_blend, image_blend,
    image_blend, image_blend, false);
```

# Variables
* All variables, including globals, are `lowercase_separated_by_underscores`.

* Global variables should be prefixed with lowercase common library prefix.

```gml
// Here ce is the library prefix
global.ce_game_settings = ds_map_create();
```

* Variables should have descriptive names. Other programmers should be able to
tell what a variable is used for based on its name.

* Abbreviations should be used sparsely. Use only commonly known ones.

* Single letter variables like `i`, `j`, `k`, etc. are allowed only in loops.
The only exception to this are variables `x`, `y`, `z` for instance position.

* All local variables (defined with `var`) must be prefixed with `_` (underscore).
The only exception to this are single letter variables used in loops.

* System variables which shouldn't be directly modified can be "hidden" from
autocomplete by prefixing them with `__` (two underscores).

```gml
// Here ce is a library prefix
__ce_components = ds_list_create();
global.__ce_objects_with_component = ds_map_create();
```

# Functions
* Prefix function arguments with an underscore `_`.

```gml
/// @func func_name(_arg[, _optional_arg])
/// @param {type} _arg
/// @param {type/undefined} [_optional_arg]
function func_name(_arg)
{
   var _optional_arg = (argument_count > 1) ? argument[1] : undefined;
   // ...
}
```

## Anonymous functions
* Opening brace of an anonymous function is on the same line.
* Function body is still Allman-style indented.

```gml
var _fn = function () {
   if ()
   {
      // ...
   }
   else
   {
      // ...
   }
};
```

## Constructors
```gml
/// @func Vec2(_x, _y)
/// @param {real} _x
/// @param {real} _y
function Vec2(_x, _y) constructor
{
   x = _x;
   y = _y;
   
   /// @func add(_vec)
   /// @param {Vec2} _vec
   /// @return {Vec2}
   static add = function (_vec) {
      x += _vec.x;
      y += _vec.y;
      return self;
   };
}
```

# Structs
* Opening brace of a struct is on the same line.

```gml
var _struct = {
   a: "Some",
   b: "Values",
   c: "Here"
};
```

# Enums
Enums follow the same rules as resources and they have their own type prefix `E`.
All enum members are `UpperCamelCase` with exception of `SIZE`, which is commonly
put as the last member to hold a size of an enum.

Always end an enum definition with a semicolon!

```gml
// Here CE_ is the prefix
enum CE_ETextureType
{
    Diffuse,
    Specular,
    Normal,
    Metallic,
    Roughness,
    AmbientOcclusion,
    SIZE
};
```

# Macros
All macro names are `CAPITALIZED_WITH_UNDERSCORES` and prefixed with capitalized
common library prefix (if there is one) followed by an `_` (underscore).

```gml
// Here CE_ is the prefix
#macro CE_MAX_PLAYER_COUNT 4
```

# If statements
* Always use parentheses in conditions.
* You can break conditions into multiple lines, like in the following example.
* Braces in if statements are mandatory, even if they surround only one
expression.

```gml
if (conditionA
    || (conditionB && conditionC))
{
    // Do something...
}
else
{
    // Do something else...
}
```

# Switch
* Cases of a switch statement should be indented on the same level as braces.
* The default case should be always defined, even if empty.

```gml
switch (value)
{
case 0:
    break;

case 1:
case 2:
    break;

default:
    break;
}
```

# Pragma
Make sure that `gml_pragma` is always the first line of a script!
