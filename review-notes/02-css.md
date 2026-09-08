# CSS — The specific gaps behind your choices

Source: `css/style.css`. Every **Your Code** example is an original excerpt. Some show single declarations when the full rule is not needed. The improvement examples **replace the named rules**. They are not instructions to add random overrides at the end of the file.

Your use of Flexbox, Grid, `gap`, CSS variables, and pseudo-elements is good enough to skip their basic definitions. I see no reason to force `isolation` into the project or rebuild it with a framework. I did not find a confirmed stacking bug that requires this.

## [1] Fonts: the CSS name does not match the requested font

❌ Problem

**Your Code**

```css
body {
  font-family: "Open Snas", sans-serif;
}
```

But `index.html` requests:

```html
href="https://fonts.googleapis.com/css2?family=Work+Sans:ital,wght@0,100..900;1,100..900&display=swap"
```

**Better Way**

If Work Sans is your choice:

```css
body {
  font-family: "Work Sans", sans-serif;
}
```

**Note**

The browser does not choose a font just because you linked its stylesheet. `font-family` asks for specific names. `Open Snas` does not match `Work Sans`. The browser also does not correct the spelling to Open Sans. The result is a `sans-serif` fallback. This can change line lengths even when your sizes are correct.

If you want Open Sans, correct **both the request and the name**, not just the name. Test the breakpoints again after choosing the real font. The measurements in this review describe the current font, not the design after the fix.

## [2] Portfolio and Testimonials: changing the box size distorts the image

❌ Problem

**Your Code**

```css
.portfolio .portfolio-box img {
  max-width: 100%;
  height: 300px;
  width: 100%;
  transition: 0.3s;
  filter: grayscale(100%);
}
```

```css
.skills .test-content .artic-content img {
  width: 150px;
  height: 150px;
  border-radius: 50%;
}
```

**Better Way**

Keep the other styles and add this to the Portfolio images:

```css
object-fit: cover;
display: block;
```

For the testimonial image, use:

```css
.skills .test-content .artic-content img {
  width: 150px;
  height: 150px;
  flex-shrink: 0;
  object-fit: cover;
  border-radius: 50%;
}
```

**Note**

The first Portfolio image is originally `1500×1000`. Its box changes width but keeps a height of `300px`. The first Testimonials image is originally `234×148`, but you display it as a square. I saw the face stretched vertically. `border-radius` only clips the edges. It does not correct the image ratio.

`object-fit: cover` keeps the content’s ratio and crops the extra parts to fill the box. `contain` keeps the whole image visible but may leave empty space. `object-position` controls the crop position when you need to keep a face visible. `display: block` removes the baseline gap of an inline image. It does not fix distortion. [MDN: object-fit](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/object-fit).

`flex-shrink: 0` stops Flexbox from reducing the testimonial image’s width under pressure. But it **does not solve the narrow-screen layout on its own**. It may leave even less room for the text. Use it together with the testimonial layout change in `03-responsive.md`.

Another option for project images, if you want a fixed ratio instead of a fixed height, is `aspect-ratio: 3 / 2; height: auto; object-fit: cover`. Do not keep `height: 300px` and expect `aspect-ratio` to override that set height.

## [3] Services and headings: what are you actually aligning?

▲ Better Approach — strengthen the understanding behind what already works

**Your Code**

```css
.services .services-container .srv-box {
  flex-direction: column;
  align-items: center;
}
```

In the grid rule inside the media query:

```css
text-align: center;
```

And in `.main-heading p`:

```css
display: inline-block;
```

**Better Way**

Keep the current Services alignment. To make it clearer which element owns the style, you can put `text-align: center` in the same `.srv-box` rule inside the media query. Remove it from the container if the other content does not need to inherit it. Remove `display: inline-block` from `.main-heading p`. Its parent is already flex.

**Note**

In Services, `align-items: center` centers **the icon’s box and the `.serv-text` box** horizontally after you use `column`. It does not set where paragraph lines sit inside that box. `text-align` works on lines. Text can inherit it even when an ancestor is flex. So “text-align does not affect flexbox” is not accurate.

Here, `justify-content` works vertically. If `.srv-box` is about as tall as its content, there is no free vertical space to distribute. It is normal to see no movement.

`.main-heading` is a flex column with `align-items: center`. So the current heading alignment does not come from `inline-block`. The parent centers its flex items. Giving a child an outer `display` of inline-block does not make it inline text inside this parent. Flex items go through blockification. Normal block layout works differently: `text-align` centers inline content, while a block with `fit-content` width needs something such as `margin-inline: auto`.

The lesson: before changing a property, ask, “Do I want to move the box itself or the content inside it? Which axis am I using?”

## [4] Design: why did you need !important?

▲ Better Approach

**Your Code**

```css
.design figcaption {
  width: 100% !important;
}
```

The base selector is:

```css
.design figure figcaption {
```

**Better Way**

Inside the same media query:

```css
.design figure figcaption {
  width: 100%;
}
```

**Note**

The current value works. Specificity is probably why you reached for `!important`. The base selector is `(0,1,2)`, and the override is `(0,1,1)`. Coming later does not beat higher specificity in this case.

Using the same selector lets the later rule win without `!important`. In future, a direct class such as `.design-caption` in both rules would be clearer. `!important` is not always forbidden. There is simply an easier local solution here. Check the cascade in DevTools before deciding that the media query is broken.

## [5] Pricing: the currency moves relative to the card, not the number

▲ Better Approach — the weak point was clear at 600px

**Your Code**

```html
<p data-price="/Mo">19</p>
```

The two positioning excerpts:

```css
left: 30%;
top: 0;
```

```css
right: 20%;
bottom: 5%;
```

**Better Way**

Make the parts of the price real content in one group:

```html
<p class="price-amount">
  <span class="currency">$</span>
  <span class="amount">19</span>
  <span class="period">/Mo</span>
</p>
```

Replace the old price paragraph and pseudo-element rules with the matching rules below:

```css
.pricing .price-amount {
  display: inline-flex;
  align-items: baseline;
  gap: 0.25rem;
}
.pricing .price-amount .amount {
  font-size: clamp(3.5rem, 6vw, 5rem);
}
.pricing .price-amount .currency {
  align-self: flex-start;
  font-size: 1rem;
}
.pricing .price-amount .period {
  font-size: 0.9375rem;
}
```

**Note**

The current `p` is a wide block that fills the card. So the `left/right` percentages do not measure distance from the number. At 600px, the numbers stay fairly small, while `$` and `/Mo` move farther away because the card gets wider. Adjusting percentages for every width repeats the problem.

The suggested group gets its width from its parts. `gap` directly controls the space between them. The baseline aligns the unit’s text line with the number’s text line. `align-self: flex-start` raises the currency if that is the intended appearance.

The current pseudo-content appeared in this browser’s accessibility tree. I will not claim that it is always unreadable. But currency and billing period are key information. CSS should not be responsible for that content. Decorations such as heading lines and circles are a better use of pseudo-elements.

Important: you already use `clamp()`. I am not presenting it as something you have never seen. The current Pricing layout and `gap` calculations are also good. There is no need to replace Flexbox with Grid just for a different style.

## [6] Testimonials: the author’s name does not need absolute

▲ Better Approach

**Your Code**

```css
.skills .test-content .text p:last-child {
  color: var(--paragraph-secondary-color);
  font-size: 13px;
  position: absolute;
  right: 0;
}
```

**Better Way**

```css
.skills .test-content .text p:last-child {
  color: var(--paragraph-secondary-color);
  font-size: 13px;
  margin-top: 0.5rem;
  text-align: right;
}
```

**Note**

The goal is to align short text to the right, not place a decoration over an element. `absolute` takes the name paragraph out of normal flow. Its height is not counted in `.text`. The current spacing makes it look acceptable with the current content. But it relies on outer padding to fit something the parent does not measure.

`text-align: right` achieves the goal and lets the parent grow if the name wraps. After removing absolute from the last paragraph, check whether another child still needs `position: relative` on `.text`. Do not keep it automatically.

## [7] Subscribe: the selector looks for an element that does not exist

❌ Problem

**Your Code**

```css
.subscribe .subscribe-text p {
  font-size: 14px;
}
```

**Better Way**

To style the current paragraph without adding a wrapper:

```css
.subscribe > .container > p {
  font-size: 14px;
}
```

**Note**

In the HTML, the paragraph is a direct child of `.container`. There is no `.subscribe-text` at all. The DOM check found zero matches for the original selector. So this is not an inheritance or specificity problem. Diagnose in this order: **Does the selector match an element? Does the rule lose in the cascade? Is the property suitable for this layout?**

Do not add a new `div` just to support a wrong selector. Do not add `!important` to a rule that matches nothing.

## [8] Subscribe: filling the background does not need height: 100%

▲ Better Approach — the current version meets the goal

**Your Code**

```css
.subscribe .input-section {
  display: flex;
  align-items: stretch;
  border: 1px solid white;
  position: relative;
}
```

And on the button:

```css
display: flex;
align-items: center;
```

**Better Way**

Keep this behavior. `stretch` is the usual default for align-items in Flexbox. Writing it here is still fine because it states your intent. Do not add a fixed height or `height: 100%` without a need.

**Note**

The current version uses a **button** for subscribing, not a `label`. The parent is a flex row. The field’s padding helps set the row’s height. The button has an automatic height, so it stretches vertically. Another flex layout inside the button centers its text vertically. That is why its background filled the form’s height in the test.

Do not use a `label` instead of a submit button just because it can have a background. `height: 100%` needs a height it can calculate from. If the parent’s height comes from its content, that percentage does not automatically mean “fill it.” The Skills bar shows the same idea: its `.prog` parent has a set height of `30px`, so `span { height: 100% }` has a clear reference.

# New Things Worth Learning From This Project

1. **object-fit / object-position**: changing image dimensions distorted their ratio.
2. **Normal flow versus positioned content**: understand the price, author name, and Video instead of memorizing left/top values.
3. **Computed styles and selector matching**: separate a rule that does not match from specificity and from a property that does not do what you expect.
4. **The containing block for percentages**: which box does `%` refer to? This explains the weak price layout and element heights.
