# Reveal-a11y Extension For Quarto

A Revealjs plugin extension for better accessibility of revealjs presentations in Quarto. This work is entirely based on [@marcysutton](https://github.com/marcysutton/reveal-a11y/tree/master)'s work.

## Installing

```bash
quarto add Nenuial/quarto-reveal-a11y
```

This will install the extension under the `_extensions` subdirectory.

## Using

Add in your YAML frontmatter:

```yaml
revealjs-plugins:
  - reveal-a11y
```

## Features

### Hidden offscreen slides

This plugin adds CSS to "really hide" offscreen slides using `display: none;` on an element wrapping each slide. This technique was used to avoid issues with transitions and `display: none`. For this to work, the styles must be loaded in HTML as a `<link>` tag (as opposed to injecting dynamically with JavaScript).

### Dynamically labeled slide sections

HTML `<section>` elements commonly used for slides will act as landmarks in screen readers. To make them easier to identify, this plugin dynamically adds an `aria-label` property with a value of "Slide 1", as an example. For nested slides, it will add "Slide 1, child 1" with numbers relative to that slide.

Purpose: uniquely labeled sections are more useful in assistive technology than `<section>` soup.

Before (your code):
```html
<section>Reveal.js</section>
<section>
	<section>It might be dated</section>
	<section>But it's still useful</section>
</section>
```

After (dynamically rendered code):
```html
<section aria-label="Slide 1">Reveal.js</section>
<section aria-label="Slide 2">
	<section aria-label="Slide 2, child 1">It might be dated</section>
	<section aria-label="Slide 2, child 2">But it's still useful</section>
</section>
```

### Dynamic Skip Links / Table of Contents 

This plugin only supports nesting of sections 2 levels deep. It injects a "Skip to Navigation" link at the beginning of the source order as well as an unordered list of slide links at the end. The content for those links is sourced from each slide: it grabs either the first child text node or the bare `textContent`. Various selectors for injected markup can be manipulated as options.

Default options (modifiable in `plugin/accessibility/helper.js`):

```javascript
new SkipLinks({
	enabled: true,

	global_skip_link_id: 'global-skip-link',

	slide_skip_links_id: 'table-of-contents',

	global_skip_link_text: 'Show navigation',

	skip_link_target_selector: '.accessibilityWrapper',

	controls_selector: '.controls'
})
```

## Example

Here is the source code for a minimal example: [example.qmd](example.qmd).

## License

MIT licensed

Copyright (C) 2015 Marcy Sutton, http://marcysutton.com