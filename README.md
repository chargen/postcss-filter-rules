# PostCSS Filter Rules

[![npm version][npm-img]][npm] [![Build Status][ci-img]][ci]

[PostCSS] plugin that filters rules with a callback function on each selector. Can be used to filter out individual rules or remove all rules besides those you wish to keep.

## Installation

```shell
npm install postcss-filter-selectors --save-dev
```


## Usage

```js
postcss([ require('postcss-filter-rules') ])
```

Or with [grunt-postcss]:

```js
grunt.initConfig({
	postcss: {
		options: {
			processors: [
				require('postcss-filter-rules')(options)
			]
		},
		dist: {
			src: 'css/*.css'
		}
	}
});
```

See [PostCSS] docs for examples for your environment.

## Options

**filter**  
Type: `Function`

Function used to filter selectors. Called for each selector in a rule.

- Selectors are kept if the function returns `true`, otherwise they are removed.
- If all of the selectors for a rule are removed, the rule is also removed.
- If all rules within an [at-rule] are removed, the block itself is also removed.

The function receives two arguments, `selector` and `parts`:

- **selector** (String): The selector, as authored in CSS
- **parts** (Array): An array of elements, classes, IDs and pseudo-classes in the selector. Can be used to quickly search for the existence of classes or IDs.  
i.e. for the selector `.a + strong.b`, the argument will contain: `[".a", "strong.b"]`

For example, to keep only the `.styleguide` class:

```js
{
	filter: function(selector, parts) {
		return parts.indexOf('.styleguide') > -1;
	}
}
```

With the input CSS:

```css
.styleguide span,
.button span {
	color: red;
}
.button {
	color: blue;
}
```

Will output:

```css
.styleguide span {
	color: red;
}
```

**keepAtRules**  
Type: `Array` (Optional)  
Default: `['charset', 'import', 'keyframes']`

By default, `@font-face` and any empty [at-rules] (after filtering) are removed. To keep specific at-rules, provide an array of names to this option. For example:

```js
{
	keepAtRules: ['font-face', 'import']
}
```

- To keep all at-rules, use the value `true`
- To discard all at-rules, use an empty array `[]`

## Todo

- Keep a `@keyframes` rule when it is referenced by `animation-name` or the `animation` shorthand.
- Keep a `@font-face` rule when it is referenced by `font-family` or the `font` shorthand.

## License

Licensed under the BSD 3-Clause License.

[grunt-postcss]: https://github.com/nDmitry/grunt-postcss
[PostCSS]:       https://github.com/postcss/postcss
[npm-img]:       https://badge.fury.io/js/postcss-filter-rules.svg
[npm]:           https://badge.fury.io/js/postcss-filter-rules
[ci-img]:        https://travis-ci.org/lachlanmcdonald/postcss-filter-rules.svg
[ci]:            https://travis-ci.org/lachlanmcdonald/postcss-filter-rules
[at-rule]:       https://developer.mozilla.org/en-US/docs/Web/CSS/At-rule
[at-rules]:      https://developer.mozilla.org/en-US/docs/Web/CSS/At-rule
