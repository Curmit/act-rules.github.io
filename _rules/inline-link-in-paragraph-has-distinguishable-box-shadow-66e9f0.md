---
id: 66e9f0
name: Inline link has distinguishable box-shadow
rule_type: atomic

description: |
  This rule checks that inline links have a box-shadow that distinguishes them from the surrounding text

input_aspects:
  - DOM Tree
  - CSS Styling

acknowledgements:
  authors:
    - Brian Bors
    - Carlos Duarte
  previous authors:
    - Wilco Fiers
---

## Applicability

This rule applies to any [visible][] HTML element that is a [semantic link][], where the closest element (including itself) that generates an [inline box][] is part of a [line box][] that has [inline boxes][inline box] generated from an element containing [visible][] [text nodes][text node] that do not have a [semantic link][] as a [descendant][] in the [flat tree][].

**Note:** The test targets are visible link elements that are part of a block of content that is displayed inline.

## Expectation

Each target element has a `box-shadow` style property with a [computed](https://drafts.csswg.org/css-cascade/#computed-value) [color value](https://drafts.csswg.org/css-backgrounds/#shadow-color) that makes it [visible][].

## Assumptions

- This rule assumes that [`br` HTML elements][br] are only used for line breaks that are actually part of the content and not for separating thematic groups of the content. 
- This rule assumes that the link is distinguishable from the rest of the text with color, which means it fails SC 1.4.1 when there is not another way to distinguish it.
- This rule assumes that the 3:1 contrast difference between text is minimal to what would be sufficient to meet WCAG 2.0. This value is part of [technique G183](https://www.w3.org/WAI/WCAG21/Techniques/general/G183), but is not specified in the [1.4.1 success criterion](https://www.w3.org/WAI/WCAG21/Understanding/use-of-color.html).
- This rule assumes that any change in font is sufficiently distinguishable, and that fonts are loaded when they are present.
- This rule assumes that if multiple colors are used in the visible text nodes of the block of content then color can not be a distinguishing factor.
- This rule assumes that if `box-shadow` is used in the different visible text nodes of the block of content then `box-shadow` can not be a distinguishing factor.

## Accessibility Support

_No accessibility support issues known._

## Background

- [Understanding Success Criterion 1.4.1: Use of Color](https://www.w3.org/WAI/WCAG21/Understanding/use-of-color.html)
- [G183: Using a contrast ratio of 3:1 with surrounding text and providing additional visual cues on focus for links or controls where color alone is used to identify them](https://www.w3.org/WAI/WCAG21/Techniques/general/G183)
- [F73: Failure of Success Criterion 1.4.1 due to creating links that are not visually evident without color vision](https://www.w3.org/WAI/WCAG21/Techniques/failures/F73)
- [C15: Using CSS to change the presentation of a user interface component when it receives focus](https://www.w3.org/WAI/WCAG21/Techniques/css/C15)

## Test Cases

### Passed

#### Passed Example 1

This is a link that is a descendant of a paragraph element, and therefore in an inline block of content. The link has a distinguishing box-shadow.

```html
<style>
	* {
		text-decoration: none;
	}
	a.test {
		text-decoration: none;
		box-shadow: 4px 4px;
	}
</style>
<p>Read about WAI on the <a class="test" href="http://w3.org/WAI">WAI webpage</a>.</p>
```

### Failed

#### Failed Example 1

This is a link that is a descendant of a paragraph element, and therefore in an inline block of content. The underline is removed and the link has no visual cues of being recognized as a link.

```html
<style>
	*,
	a.test {
		text-decoration: none;
	}
</style>
<p>Read about WAI on the <a class="test" href="http://w3.org/WAI">WAI webpage</a>.</p>
```

### Inapplicable

#### Inapplicable Example 1

There is no [semantic link][] element.

```html
<p>Read about WAI on the <u>underlined text</u>.</p>
```

#### Inapplicable Example 2 

This link is not [visible][].

```html
<p>Read about WAI on the <a href="http://w3.org/WAI" style="visibility:hidden">WAI webpage</a>.</p>
```

#### Inapplicable Example 3

This link is the only content in the inline block of content it belongs to.

```html
<p><a href="http://w3.org/WAI">WAI webpage</a></p>
```

#### Inapplicable Example 4

Each link is the only content in the inline block of content it belongs to.

```html
<ul>
	<li><a href="https://www.w3.org/WAI/WCAG21/Understanding/non-text-content.html">SC 1.1.1</a></li>
	<li><a href="https://www.w3.org/WAI/WCAG21/Understanding/audio-only-and-video-only-prerecorded">SC 1.2.1</a></li>
</ul>
```

#### Inapplicable Example 5

There are only [semantic links][semantic link] in the inline block of content.

```html
<p><a href="https://www.w3.org">W3C </a><span role="link" onclick="location='https://www.w3.org/WAI/'">WAI</span></p>
```

[br]: https://html.spec.whatwg.org/#the-br-element
[descendant]: https://dom.spec.whatwg.org/#concept-tree-descendant
[flat tree]: https://drafts.csswg.org/css-scoping/#flat-tree 'Definition of flat tree'
[inline box]: https://drafts.csswg.org/css-display/#inline-box 'Definition of inline box'
[line box]: https://drafts.csswg.org/css2/visuren.html#line-box 'Definition of line box'
[semantic link]: #semantic-link 'Definition of semantic link'
[text node]: https://dom.spec.whatwg.org/#text
[visible]: #visible 'Definition of visible'