# Clean Code — A separate review from how the interface works

These are maintenance improvements for after the functional, responsive, and accessibility issues. They are not a reason to rewrite the project. Here, I separate “it works” from “it is easy to change.” I did not edit vendor files, notes, or your other files.

## [1] A long selector ties styling to the full path to an element

▲ Better Approach

**Your Code**

```css
.skills .our-skills .content .prog-holder .prog span {
  display: block;
  height: 100%;
  background-color: var(--main-color);
  position: relative;
}
```

**Better Way**

If you keep the custom bar interface instead of choosing meter:

```html
<span class="skill-bar-fill" style="width: 90%" data-progress="90%"></span>
```

```css
.skill-bar-fill {
  display: block;
  height: 100%;
  background-color: var(--main-color);
  position: relative;
}
```

**Note**

The old selector works. But adding a wrapper or moving the bar may break its styling because it describes the whole tree. A clear class names the role directly and lowers specificity. That makes a local override easier later. Apply the same idea to `.srv-box .serv-text` and Pricing when needed. You do not need to rename the whole project at once.

I am not criticizing `span` for being generic. It fits a visual part inside a component. The discussion about the meaning of a Skills value in `01-html.md` is separate. Do not implement both the meter suggestion and the custom bar suggestion without a reason.

## [2] The active state should not depend on an item’s number

▲ Better Approach

**Your Code**

```css
.portfolio header li:nth-child(1),
.portfolio header li:hover {
  background-color: var(--transparent-color);
  color: white;
}
```

**Better Way**

With the current HTML, the class name you already have is clearer:

```css
.portfolio .shuffle li.active,
.portfolio .shuffle li:hover {
  background-color: var(--transparent-color);
  color: white;
}
```

**Note**

You wrote `class="active"` in HTML, but CSS selects “the first item” instead of “the active item.” The current result matches because All is first. But it will differ if the item order or active state changes.

If you move the interaction to buttons, as described in accessibility, style `[aria-pressed="true"]` or the state class on the button itself. Update it with the behavior. Choose a clear source for state. Do not change a class while the color is still tied to item order.

## [3] CSS variables: a good start, with clearer names and cleanup needed

▲ Better Approach

**Your Code**

```css
--paragprah-main-color: #353235;
--paragraph-secondary-color: #a8a8a8;
```

```css
--paragraph-line-height: 2;
--heading-line-height: 1.6;
```

**Better Way**

```css
--text-primary: #353235;
--surface-dark: #1f2021;
```

Keep the line-height variables only if you will use them for a shared design choice. Otherwise, remove the unused definitions.

**Note**

`--paragprah-main-color` has a spelling mistake, but its references match. So it does not currently break anything. A clearer name reduces future mistakes, as long as you update every use when renaming it. The lesson is not that every typo stops CSS. Compare this with font-family, which really requested a resource under a different name.

The search showed that both line-height variables are defined but never used. Their presence suggests that changing them will change typography, but nothing will happen. In contrast, `#1f2021` is a repeated dark background with a shared meaning, so it is worth a token. The secondary text color needs separate background roles, as explained in accessibility, not just a new name.

Do not create a variable for every `10px` and `15px`. A repeated number does not always represent one design choice.

## [4] Remove leftover experiments, not code that explains your intent

▲ Better Approach

**Your Code**

```css
/* display: flex; */
```

The Services media query also repeats:

```css
margin: 0 auto;
```

The same value is already in the base rule.

**Better Way**

Remove the disabled declaration if it no longer explains a decision. Remove the override that repeats the base value. Keep the section start and end comments. They make a long file easier to navigate.

**Note**

Git keeps earlier experiments. You do not need to keep them as comments in the runtime stylesheet. Other examples are `display: inline-block` on the main-heading description’s flex item, and the `.subscribe-text` selector that matches no element. They are explained in `02-css.md`. I will not count them again as new errors.

I would not remove `align-items: stretch` just because it is a default if it explains an important goal in Subscribe. The aim is less confusion, not the fewest lines.

## [5] An unspecified transition may animate changes you did not intend

▲ Better Approach

**Your Code**

```css
transition: 0.3s;
```

This pattern repeats in navigation and Portfolio.

**Better Way**

For an image that changes only filter and transform:

```css
transition: filter 0.3s, transform 0.3s;
```

**Note**

The current form applies a transition to every animatable property when it changes. It does not mean “only the hover properties I remember.” Naming the properties protects you from unexpected width or padding animation after a future edit.

I am not claiming that this caused measured slowness. You already have a better example in `.pricing-link`, where you named background-color and color. Reuse the principle when needed. A transition alone does not animate the `display` switch as used in the header menu.

## [6] What does not need abstraction now

▲ Better Approach — keep the current simplicity

**Your Code**

```html
<header class="main-heading">
  <h2>Pricing</h2>
```

```css
.pricing .pricing-content {
  display: flex;
  flex-wrap: wrap;
  gap: 40px 24px;
}
```

**Better Way**

Keep clear shared components such as `.container`, `.main-heading`, and `.pricing-link`. Keep grouping code by sections. Repeating four HTML cards in a static page is fine. I am not asking for React or a template engine just to remove that repetition.

**Note**

Repeated section padding suggests a shared choice, and you already control it with a variable. You do not need a utility class for every property now. Choose a shared component when **the role and behavior** repeat, not just because two lines look similar.

Names such as `.text` and `.content` make sense in a small context, but they are general. As the project grows, use role names such as `.testimonial-author`. Text errors such as `Adope`, `Feature N0 1`, and `All Right Reserved` need proofreading before publishing. One CSS file is not wrong for this size. There is no reason to add a build pipeline just to look professional.

# New Things Worth Learning From This Project

1. **Low-specificity component classes**: reduce the link between styling and DOM structure.
2. **State selectors**: active is a state, not an item’s position.
3. **Meaningful design tokens**: especially backgrounds and colors by context, with unused tokens removed.
4. **Small refactoring steps**: change one idea, then check it. Avoid a large cleanup that changes the design by accident.
