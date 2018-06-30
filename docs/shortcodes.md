---
subtitle: Shortcodes
relatedKey: shortcodes
relatedTitle: Template Shortcodes
tags:
  - docs-config
  - related-filters
  - related-custom-tags
  - related-nunjucks
  - related-liquid
---
# Shortcodes

{% addedin "0.5.0" %}

Various template engines can be extended with shortcodes to for easy reusable content. This is sugar around Template Language [Custom Tags](/docs/custom-tags/). Here’s an example:

{% raw %}
```html
<!-- Nunjucks -->
{% user firstName, lastName %}
```

```html
<!-- Liquid, careful no comma between arguments -->
{% user firstName lastName %}
```
{% endraw %}


Supported in Liquid and Nunjucks templates.

```js
module.exports = function(eleventyConfig) {
  // Liquid Shortcode
  eleventyConfig.addLiquidShortcode("user", function(firstName, lastName) { … });
  
  // Nunjucks Shortcode
  eleventyConfig.addNunjucksShortcode("user", function(firstName, lastName) { … });
  
  // Universal Shortcodes (Adds to Liquid and Nunjucks)
  eleventyConfig.addShortcode("user", function(firstName, lastName) { … });
};
```

A shortcode returns content (a JavaScript string or template literal) that is injected into the template. You can use these however you’d like—you could even think of them like reusable components.

Read more about using shortcodes on the individual Template Language documentation pages:

{% templatelangs templatetypes, page, ["njk", "liquid"], "#shortcodes" %}

## Paired Shortcodes

The shortcodes we saw above were nice, I suppose. But really, they are not all that different from a filter. The real ultimate power of Shortcodes comes when they are Paired. Paired Shortcodes have a start and end tag—and allow you to nest other template content inside!

{% raw %}
```html
<!-- Nunjucks -->
{% user firstName, lastName %}
  Hello {{ someOtherVariable }}.
  
  Hello {% anotherShortcode %}.
{% enduser %}
```

```html
<!-- Liquid -->
{% user firstName lastName %}
  Hello {{ someOtherVariable }}.
  
  Hello {% anotherShortcode %}.
{% enduser %}
```
{% endraw %}

When adding paired shortcodes using the Configuration API, the first argument to your shortcode callback is the nested content.

```js
module.exports = function(eleventyConfig) {
  // Liquid Shortcode
  eleventyConfig.addPairedLiquidShortcode("user", function(content, firstName, lastName) { … });
  
  // Nunjucks Shortcode
  eleventyConfig.addPairedNunjucksShortcode("user", function(content, firstName, lastName) { … });
  
  // Universal Shortcodes (Adds to Liquid and Nunjucks)
  eleventyConfig.addPairedShortcode("user", function(content, firstName, lastName) { … });
};
```

## Universal Shortcodes

Universal shortcodes are added in a single place and subsequently available to multiple template engines, simultaneously. This is currently supported in Nunjucks and Liquid.

```js
module.exports = function(eleventyConfig) {
  // Universal Shortcodes (Adds to Liquid and Nunjucks)
  
  // Single Universal Shortcode
  eleventyConfig.addShortcode("myShortcode", function(firstName, lastName) { … });
  
  // Paired Universal Shortcode
  eleventyConfig.addPairedShortcode("user", function(content, firstName, lastName) { … });
};
```