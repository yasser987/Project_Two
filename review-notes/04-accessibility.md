# Accessibility — What can the user actually use?

I checked the DOM, accessibility tree, Tab and Enter navigation, and computed focus styles. I did not run a real screen reader such as NVDA or do a full WCAG audit. I do not claim that the site is compliant. Having no JavaScript is understandable in an HTML/CSS exercise. But visual-only controls must be separated from complete features.

## [1] Header: hover is not a menu’s open state

❌ Problem

**Your Code**

```css
.web-header .container nav .toggle-menu:hover + ul {
```

**Better Way**

For a simple HTML/CSS version, you can keep the navigation links visible and let them wrap on phones. Or use a native disclosure, such as:

```html
<details class="mobile-nav">
  <summary>Menu</summary>
  <nav aria-label="Mobile navigation">
    <a href="#services">Services</a>
    <a href="#portfolio">Portfolio</a>
    <a href="#about">About</a>
    <a href="#contact">Contact</a>
  </nav>
</details>
```

**Note**

In the 375 test, Tab reached `.toggle-menu`. Enter did not open the `ul`. The next Tab skipped the hidden links and went to Search. The cause is that `:hover` depends on the pointer, not focus or an open state. The selector also checks hover on the button itself, not whether the pointer stays over the menu.

`details/summary` provide a native open state that works with the mouse and keyboard without JavaScript. The example **replaces the current mobile structure**. It needs styling and coordination with the desktop version. It is not an extra duplicate menu that stays visible. If you later choose a button with JavaScript, update `aria-expanded` to match the real open state and provide `aria-controls`. ARIA alone does not make the menu work.

Do not treat adding `:focus-within` alone as the end of the work. After that, test touch, closing the menu, and moving between links. The 768 bug is a separate issue documented in the responsive file.

## [2] A visible icon does not give a control a name

❌ Problem

**Your Code**

```html
<button type="button" class="search-icon">
  <i class="fa-brands fa-sistrix"></i>
</button>
```

**Better Way**

```html
<button type="button" class="search-icon" aria-label="Search">
  <i class="fa-brands fa-sistrix" aria-hidden="true"></i>
</button>
```

**Note**

Search and toggle-menu appeared as buttons without a clear name in the accessibility tree. The icon is a shape. `aria-label` gives a name when there is no visible text. Here, `aria-hidden` is only for the decorative icon, not the button.

This fixes the name. It **does not create search behavior**. The current version has no search feature. You can remove the control from the demo or state that it is not implemented, instead of suggesting that it works. `cursor: text` on Search also suggests an editable field. Choosing a cursor does not change semantics.

Footer has four `i` elements without links. If they are meant to be social links, put each icon inside an `a` with a real account destination and a name such as `aria-label="Facebook"`. I will not invent account links for you. If they are only decorative, hide them from accessibility and do not present them as interactive. Services and Stats icons can be hidden from the tree because nearby text gives their meaning.

## [3] Landing and Portfolio: cursor pointer does not turn li or i into a control

❌ Problem — if these are meant to be slideshow/filter features

**Your Code**

```html
<li class="active">All</li>
<li>App</li>
<li>Photography</li>
<li>Web</li>
<li>Print</li>
```

**Better Way**

A filter selection interface, when you implement its behavior:

```html
<li><button type="button" aria-pressed="true">All</button></li>
<li><button type="button" aria-pressed="false">App</button></li>
```

**Note**

The current list items are not part of normal Tab navigation and do not filter anything. The same applies to the Landing arrows and the bullets in Landing and Testimonials. Do not fix this by adding `tabindex="0"` to every `li`. You would still need to build the keyboard behavior that a `button` already provides.

The example only becomes a working filter after you connect an event and update both the content and `aria-pressed`. This is a suitable JavaScript task later. If the project is only a static page, say that clearly. Do not count these parts as complete features. The `button > a` nesting is explained once in `01-html.md`.

## [4] Forms: a placeholder is not a lasting label

❌ Problem

**Your Code**

```html
<input name="mail" type="email" placeholder="Your Email" />
```

**Better Way**

```html
<label for="contact-email">Email address</label>
<input
  id="contact-email"
  name="email"
  type="email"
  autocomplete="email"
  placeholder="name@example.com"
/>
```

**Note**

Neither form has labels. This browser used the placeholder as a fallback name for some fields. So saying “the fields have no accessible name at all” would be inaccurate. The confirmed problem is that the title disappears when the user types. There is no explicit, lasting label connection.

You previously completed the `for/id` example in T001. The task here is to use the same knowledge in Contact and Subscribe, not memorize a new definition. A `label` describes the question. A placeholder is an optional example answer. Do not turn the Subscribe button into a label. Do not use absolute positioning to put a label inside a field just to copy a placeholder.

## [5] Forms: removing outline hid the navigation position

❌ Problem

**Your Code**

```css
.contact input[type="text"]:focus,
.contact input[type="email"]:focus,
.contact textarea:focus {
  outline: none;
}
```

And in the Subscribe field:

```css
.subscribe .input-section input:focus {
  outline: none;
}
```

**Better Way**

Replace the rules that remove focus with a clear indicator, such as:

```css
.contact input:focus-visible,
.contact textarea:focus-visible {
  outline: 2px solid #222;
  outline-offset: 3px;
}
.subscribe .input-section input:focus-visible {
  outline: 2px solid white;
  outline-offset: -4px;
}
```

**Note**

When I used Tab to reach the email field, the computed outline was `none`. There was no replacement box-shadow. A small caret does not always make the active field clear enough. You already have a good pattern in `.pricing-link:focus-visible`. Reuse **the principle**, with colors that suit each background. Do not put a dark indicator on a dark section.

The example replaces the old `:focus` rules. If you keep them, some more specific selectors may win. A visible indicator is required for users who navigate with a keyboard. [W3C: WCAG 2.2, Focus Visible](https://www.w3.org/TR/WCAG22/#focus-visible).

For `textarea`, `resize: none` stops the user from making the message area larger, even for a long message. `resize: vertical` is a useful improvement alongside the existing `min-height: 200px`. I am not claiming that data is currently lost, because internal scrolling is available.

## [6] Colors: the secondary text is too faint

❌ Problem

**Your Code**

```css
--main-color: #21cefc;
--transparent-color: rgb(15 116 143/70%);
--paragprah-main-color: #353235;
--paragraph-secondary-color: #a8a8a8;
```

And for `.main-heading` paragraphs:

```css
color: #c7c3c5;
```

**Better Way**

Separate the text color on white backgrounds from the text color on dark backgrounds. For example:

```css
:root {
  --text-muted-on-light: #767676;
}
.main-heading p {
  color: var(--text-muted-on-light);
}
.contact .contact-form button,
.subscribe .input-section .subscribe-button {
  color: #222;
}
```

**Note**

Calculating contrast from the solid CSS colors gave:

| Color pair | Approximate contrast | Location |
| --- | ---: | --- |
| `#c7c3c5` on white | 1.74:1 | Heading descriptions |
| `#a8a8a8` on white | 2.38:1 | Services, Testimonials, and Contact information |
| White on `#21cefc` | 1.86:1 | Subscribe, Send Message, and More buttons |
| `#767676` on white | 4.54:1 | Example alternative for secondary text |
| `#222` on `#21cefc` | 8.56:1 | Example alternative for button text |

Normal text usually needs 4.5:1 under WCAG AA. Large text, as defined by the standard, needs 3:1. The first three color pairs fall below both. This is not a matter of taste in gray. Cyan on white also appears in Services headings and Portfolio descriptions. [W3C: Contrast Minimum](https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum.html).

Do not change the secondary token globally to dark gray and break its use on dark sections. Separate its role by background. These numbers are not an audit of every pixel over images and transparency. Those backgrounds need separate measurements. The dark Kasper logo is also hard to see in Footer. But I do not mix the standard’s logo exceptions with normal interface text.

## [7] Portfolio: empty alt hides the work itself

❌ Problem

**Your Code**

```html
<img src="images/portfolioImageOne.jpg" alt="" />
```

**Better Way**

```html
<img
  src="images/portfolioImageOne.jpg"
  alt="Snow-covered mountains reflected in a lake"
/>
```

**Note**

Here, the image is the work being shown. `Awesome Image / Photography` does not describe it as an alternative. `alt=""` suits a decorative image, but here it hides the work from a user who cannot see it. Give each work a description that serves its purpose, not its file name or a list of keywords.

Not every empty alt is wrong. Phones and About screens can be treated as decoration alongside the existing text. A testimonial author’s image may not need an extra description if the name is shown and the image adds no needed information. The logo already has alt text, which is good.

Video has controls, but the overlay hides them on phones, as documented in the responsive file. There is no `track`. I did not verify audio or speech that needs captions, so I cannot confirm a speech-caption problem. If the video contains important audio information, add correct captions, not an empty file. The performance file covers autoplay behavior and its trade-offs.

# New Things Worth Learning From This Project

1. **Native disclosure or a stateful menu**: opening is a state, not a hover effect.
2. **Accessible name**: text or a label gives meaning. The icon is not the name.
3. **Focus-visible and contrast by background**: these are part of a component, not final decoration.
4. **Informative versus decorative**: the alt choice depends on the image’s role on the page.
