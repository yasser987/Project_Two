# Responsive — Watch the limits of your content, not device names

The site was served locally at `http://127.0.0.1:8767/` and checked in the environment’s Chromium browser on 2026-09-08. These results describe the original version, not the suggested fixes.

I took screenshots, checked sections, and read element sizes and computed styles. I visually checked the full page at 1440 and 375. I checked selected parts at 320, 768, 1024, and 1280, and measured layout at all these widths. Viewport tests are not tests on a real device or in Safari/iOS.

## Measurement results before the lessons

The main test height was `900px`. In this environment, `clientWidth` is 15px less than the viewport because of the scrollbar.

| Viewport | Page area width | scrollWidth | Main result |
| --- | ---: | ---: | --- |
| 320 | 305 | 415 | Portfolio goes off screen; Testimonials are cramped; Video caption is taller than the video |
| 375 | 360 | 415 | Same overflow; menu depends on hover; Video controls are behind the caption |
| 768 | 753 | 753 | Both menu and links disappear; Stats shows four unequal cards instead of the tablet layout |
| 1024 | 1009 | 1009 | No measured horizontal overflow; Pricing has four cards and Skills has two columns |
| 1280 | 1265 | 1265 | No measured horizontal overflow; Portfolio has two columns |
| 1440 | 1425 | 1425 | No measured horizontal overflow; desktop layout is generally consistent |

I also tested `429, 430, 444, 445, 600, 767, 769, 991, 992, 1005, 1006`. I tested a short height at `375×400` too. No horizontal overflow does not mean that text is comfortable to read or that features work.

## [1] Portfolio: the minimum is larger than the available space

❌ Problem

**Your Code**

```css
.portfolio .portfolio-content {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 100px;
}
```

**Better Way**

Change only the column definition. Keep the gap if it matches your intent:

```css
grid-template-columns: repeat(auto-fill, minmax(min(100%, 400px), 1fr));
```

**Note**

The cause is not that `auto-fill` failed to create one column. It did create one column, but it cannot go below the `400px` minimum. At a 375 viewport, the grid’s inner space is smaller than that.

In the fix, `min(100%, 400px)` chooses the smaller value. In a narrow container, the minimum becomes the container’s width. In a wide container, it stays at 400px. The rest of Grid’s behavior stays the same, so this problem does not need a new media query. This is a small addition to your current use, not another long Grid lesson. [MDN: min()](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/min).

Boundary measurements in this environment: at 429, overflow was 1px. At 430, page overflow disappeared, but the card still went past the container’s inner space. At 444, the grid had `399px` of space and the card was `400px`. At 445, the space became `400px` and the card fit. So **a missing scrollbar does not prove that an element fits its container**. The scrollbar affects these boundaries. The container’s width is what matters.

Do not use `overflow-x: hidden` on `body`. It would clip your work instead of fixing the cause. I did not change the page to test the fix. Measure again after you apply it yourself.

## [2] Portfolio filters: fix the group’s wrapping too

❌ Problem

**Your Code**

```css
.portfolio header .shuffle {
  display: flex;
  margin-top: 30px;
  border: 1px solid #d5d0d0;
  border-radius: 6px;
}
```

**Better Way**

Add this to the same rule:

```css
flex-wrap: wrap;
justify-content: center;
max-width: 100%;
```

**Note**

The five words and their padding do not fit in one row on a phone. I saw the group go beyond its space. `display: flex` uses `nowrap` by default. Fixing the image columns does not fix this separate group.

The choice is between wrapping into two rows and allowing horizontal scrolling for a longer list. Wrapping is simpler for this small number of items. Do not keep reducing the font to squeeze the design into one line. Making these into real controls is a separate topic in `04-accessibility.md`.

## [3] Header and Stats: media query boundaries overlap in one place and leave a gap in another

❌ Problem

**Your Code**

Header: `min-width: 768px` hides the menu button, while `max-width: 768px` hides the `ul`:

```css
@media (min-width: 768px) {
  .web-header .container .toggle-menu {
    display: none;
  }
}
```

```css
@media (max-width: 768px) {
  .web-header .container nav ul {
    display: none;
  }
```

And in Stats:

```css
@media (max-width: 767px) {
  .stats .stats-box {
    flex-basis: 100%;
  }
}
@media (min-width: 769px) {
  .stats .stats-box {
    flex-basis: 50%;
  }
}
```

**Better Way**

Use a mobile base, then clear overrides as width increases. This avoids separate min/max pairs. This Stats example replaces the three current media queries:

```css
.stats .stats-box {
  flex-basis: 100%;
}
@media (min-width: 768px) {
  .stats .stats-box {
    flex-basis: 50%;
  }
}
@media (min-width: 992px) {
  .stats .stats-box {
    flex-basis: 25%;
  }
}
```

Use the same idea for Header: a mobile base, then one rule at the chosen boundary that shows desktop links and hides the toggle. The accessibility file covers making the mobile menu work. Fixing the numbers alone does not fix hover-only behavior.

**Note**

In Header, `min` and `max` both include the boundary value. At 768, both hiding rules apply. In Stats, neither rule covers 768. So `flex-basis` returns to `auto`, and the content sets the item widths. I saw a row of four unequal cards. At 767, each card had its own row. At 769, there were two columns.

There is no single required value for every section. I do not suggest making all breakpoints the same automatically. **Each component needs defined behavior at every width.** Check the boundary and the widths just before and after it. Do not rely on `767px` alone to cover all fractional values caused by zoom.

## [4] Video: the overlay is larger than what it covers

❌ Problem

**Your Code**

```css
.clip {
  position: relative;
}
.clip figure video {
  width: 100%;
}
```

And in figcaption:

```css
position: absolute;
top: 50%;
left: 0;
transform: translateY(-50%);
width: 100%;
```

**Better Way**

The simplest choice for a video with controls is a caption in normal flow below the video on phones. This replaces the behavior inside the current media query:

```css
.clip figure video {
  display: block;
}
@media (max-width: 768px) {
  .clip figcaption {
    position: static;
    transform: none;
    padding: 30px 15px;
  }
}
```

**Note**

At 320, the video is about `155px` high and the caption is `220px`. At 375, the video is about `186px` and the caption is `220px`. The caption is out of flow, so it does not make the section grow to fit it. It extends beyond the video and covers the native controls. Repeatedly reducing the font is not the solution.

There is a separate desktop design choice. The caption’s width is relative to `.clip`, its nearest positioned ancestor. The video sits in a narrower `.container`. So the strip extends beyond both sides of the video. **This may be intentional**, so I do not call it a bug on its own. If you want it to match the figure, make `.clip figure` the positioned ancestor. Watch the container’s padding. Decide on the design before changing position.

## [5] Testimonials: keeping text on screen is not enough

▲ Better Approach — cramped reading at 320–375

**Your Code**

```css
.skills .test-content .artic-content {
  display: flex;
  gap: 40px;
  align-items: center;
  margin-bottom: 40px;
  border-bottom: 1px solid var(--paragraph-secondary-color);
  padding-bottom: 40px;
}
```

**Better Way**

Start with a range such as 600px. Then adjust it based on readability with the final font:

```css
@media (max-width: 600px) {
  .skills .test-content .artic-content {
    flex-direction: column;
    gap: 20px;
    text-align: center;
  }
}
```

**Note**

The `150px` image and `40px` gap use most of the phone’s width before the text gets any space. I saw very short lines at 320. Making `.skills .container` a column changes the relationship between Testimonials and Our Skills. It does not change the layout inside `.artic-content`. This is an important example of **two different levels of Flexbox**.

600 is not “the exact required breakpoint.” It is a reasonable starting point between the cramped width and the more comfortable width that was checked. Test again after fixing the font. Image distortion and the author name’s position are explained in `02-css.md`. Do not mix the three problems into one change.

## [6] Landing: test height too

▲ Better Approach — a weak point in short viewports

**Your Code**

```css
min-height: 100vh;
```

And for the text box:

```css
position: absolute;
left: 0;
top: 50%;
transform: translateY(-50%);
```

**Better Way**

This approach keeps the text in flow and lets the hero grow. It replaces the previous text positioning while keeping the text’s internal styles:

```css
.landing {
  min-height: 100vh;
  display: grid;
  align-items: center;
  padding: 130px 0 60px;
}
.landing .text {
  position: relative;
  top: auto;
  left: auto;
  transform: none;
}
```

**Note**

At `375×400`, the hero stayed 400 high. The text box started about 24 from the top, while the header was 97 high. The text box entered the header area. This shows that the layout does not reserve space for the header. `min-height` does not count an absolute child’s height to grow around it.

The values 130 and 60 in the example make space for the header and bullets. They are design choices to adjust, not proof that the example was tested. Keep decorations absolute where needed. Let the main content take part in the height calculation. Test short landscape views and zoom later, not just width.

## What did not need a forced fix

- Services does change to centered content on phones. Combining element alignment and text alignment is correct here.
- Pricing changes from 1 to 2 to 4 cards and calculates the gaps correctly. The problem is in the price parts, not the card layout.
- Subscribe’s button background fills the row height at the tested sizes. No extra height is needed.
- Contact switches between vertical and horizontal layouts. I did not find separate overflow from it during the test.
- The bottom crop of the About image looks intentional. `overflow: hidden` on a decorative component is not the same as hiding page overflow to hide a bug. The link between `bottom` and negative margin needs care if the content changes, but it is not a confirmed bug here.
- The 100 gap between Portfolio images and the 100 section padding feel long on phones. But they are visual choices, not CSS errors by themselves. You can reduce them if you want tighter spacing.

# New Things Worth Learning From This Project

1. **Limits within limits**: the Grid minimum must respect a narrower container.
2. **Breakpoint boundary testing**: test at the boundary and just before and after it, as the 768 case showed.
3. **Component-level responsiveness**: changing the outer parent does not rearrange every descendant.
4. **Content-driven height**: keep the main text in flow instead of squeezing it into a fixed overlay.
