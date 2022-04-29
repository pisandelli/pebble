# Pebble
A tiny, Stylus-powered design-token led utility class generator.  
This project is heavily inspired by [Gorko](https://github.com/hankchizljaw/gorko) but we are not that much (yet) :)

## Getting Started
Pebble takes advantage of [Stylus capability to read and parse JSON files](https://stylus-lang.com/docs/bifs.html#jsonpath-options).  
The main idea behind Pebble is to read a Design-Token in JSON format and then generate your utility classes.

## Design Token
The design token must follow some rules to be correctly parsed. Above we have an example:

```json
{

  "$scale" : {
    "300"  : "calc(1.6rem * 0.8)",
    "400"  : "1.6rem",
    "500"  : "calc(1.6rem * 1.25)",
    "600"  : "calc(1.6rem * 1.5625)",
    "700"  : "calc(1.6rem * 1.875)",
    "900"  : "calc(1.6rem * 2)"
  },

  "box" : {
    "items": {
      "block" : "block",
      "flex" : "flex",
      "hide" : "none",
      "show" : "inherit"
    },
    "prop" : "display"
  },

  "font" : {
    "items" : "$scale",
    "prop"  : "font-size"
  },

  "stack": {
    "items": {
      "300": 0,
      "400": 10,
      "500": 20,
      "600": 30,
      "700": 40
    },
    "prop": "z-index"
  },

  "weight": {
    "items": {
      "light": "300",
      "regular": "400",
      "bold": "700"
    },
    "output": "standard",
    "prop": "font-weight"
  }
}
```
## Maps
The toke can use what we call maps (maybe this name will change... later). All maps must star with a `$` sign. This indicate that you can reuse this object to populate as many [items](#items) you desire. This is not a required field. Feel free to create as many you like.

```json
{
  "$scale" : {
    "300"  : "calc(1.6rem * 0.8)",
    "400"  : "1.6rem",
    "500"  : "calc(1.6rem * 1.25)",
    "600"  : "calc(1.6rem * 1.5625)",
    "700"  : "calc(1.6rem * 1.875)",
    "900"  : "calc(1.6rem * 2)"
  }
}
```
## Rules
Rules are the core of Pebble. Here we can create the rules for our utility classes. Bellow we have the model of a rule and what it generates.  

```json
{
  "rule" : {
    "items": {
      "identifier" : "value",
      [...]
    },
    "prop" : "property"
  }
}
```
This yelds the following CSS 
```css
.rule-identifier {
  property: value;
}
[...]
```
### Items
Items can be either `objects` or `strings`. Use strings **only** for referencing a [map](#maps). For example:   
```json
{
  "rule" : {
    "items": "$scale",
    "prop" : "property"
  }
}
```
This will loop through the `$scale` map and generate the classes.

### Value
You can use `identifier.value` as `string` or **any** of [Stylus' Built-in functions](https://stylus-lang.com/docs/bifs.html). For example:

```json
{
  "foo" : {
    "items" : {
      "bar" : "round(5.4px)"
    },
    "prop" : "width"
  }
}
```
This yelds
```CSS
.foo-bar {
  width: 6px;
}
```
## Returning a String
If you want `identifier.value` as a `String` you can use [Stylus' Join](https://stylus-lang.com/docs/bifs.html#joindelim-vals). For example:  
```json
{
  "font" : {
    "items" : {
      "primary" : "join(' ', Helvetica)"
    },
    "prop" : "font-family"
  }
}
```
This yelds
```CSS
.font-primary {
  font-family: "Helvetica";
}
```
# Future Improvements
We are working on further improvements like add the `output` as `standard` or `responsive` so we can add media queries and generate the utility classes for them.

# What Pebble can do right now?
Using the above example of Design Token we can generate the follow CSS.

```CSS
.box-block {
  display: block;
}
.box-flex {
  display: flex;
}
.box-hide {
  display: none;
}
.box-show {
  display: inherit;
}
.font-300 {
  font-size: calc(1.6rem * 0.8);
}
.font-400 {
  font-size: 1.6rem;
}
.font-500 {
  font-size: calc(1.6rem * 1.25);
}
.font-600 {
  font-size: calc(1.6rem * 1.5625);
}
.font-700 {
  font-size: calc(1.6rem * 1.875);
}
.font-900 {
  font-size: calc(1.6rem * 2);
}
.stack-300 {
  z-index: 0;
}
.stack-400 {
  z-index: 10;
}
.stack-500 {
  z-index: 20;
}
.stack-600 {
  z-index: 30;
}
.stack-700 {
  z-index: 40;
}
.weight-light {
  font-weight: 300;
}
.weight-regular {
  font-weight: 400;
}
.weight-bold {
  font-weight: 700;
}
```
