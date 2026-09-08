# HTML — From appearance to meaning and behavior

Review of the local version on 2026-09-08. Source: `index.html`, without changes. The **Better Way** examples are for learning. Some need matching CSS selectors to be updated. They are not applied changes or complete features.

You already use `header`, `nav`, `main`, `section`, `figure`, `figcaption`, `blockquote`, and `address`. So the lesson is not “learn semantic HTML from the start.” It is choosing the right element within this structure.

## [1] Portfolio and Video: a link inside a button is not a better-looking button

❌ Problem

**Your Code**

```html
<button><a href="#" class="portfolio-link">More</a></button>
```

The same problem appears here:

```html
<button><a href="#">See More</a></button>
```

**Better Way**

If the goal is to move to existing content:

```html
<a href="#portfolio" class="portfolio-link">View portfolio</a>
```

If the goal is to load more items within the page:

```html
<button type="button" class="portfolio-link">Load more projects</button>
```

**Note**

The cause: an `a` with `href` and a `button` are both interactive content. Combining them creates two nested controls instead of one. Both also appeared in the accessibility tree. The rules for `button` do not allow this nesting. [MDN: button](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/button).

Choose **the behavior first**, then style the element with CSS. The second option needs JavaScript later. Writing a `button` alone does not load anything. The first option needs `id="portfolio"`, as shown in the next note. Move the `.container button a` styles to the chosen element’s class. Do not keep a wrapper only to support an old selector.

A good point: the Pricing links are not inside buttons. You do not need to turn them into buttons if they will lead to a purchase page.

## [2] Header and Pricing: a link’s text does not set its destination

❌ Problem

**Your Code**

```html
<li><a href="#">Services</a></li>
<li><a href="#">Portfolio</a></li>
<li><a href="#">About</a></li>
<li><a href="#">Contact</a></li>
```

**Better Way**

```html
<li><a href="#services">Services</a></li>
<li><a href="#portfolio">Portfolio</a></li>
<li><a href="#about">About</a></li>
<li><a href="#contact">Contact</a></li>
```

Also add the IDs to the existing opening tags. For example:

```html
<section class="services" id="services">
```

**Note**

All 13 links on the page currently use `href="#"`. To the browser, this does not mean “I have not set the feature yet.” It is an empty fragment that leads to the top of the page. A `class` helps select and style an element. A link fragment looks for a matching `id`.

Link `Contact Us` in Pricing to `#contact`. `Buy Now` needs a real destination or a defined purchase action. I am not suggesting that you invent a checkout for an HTML/CSS project. State that buying is not implemented if you present this as a static demo. Do not use `javascript:void(0)` to hide a missing feature.

## [3] Subscribe and Contact: a form’s appearance is not a sending system

❌ Problem — an incomplete feature, not proof of weak backend skills

**Your Code**

```html
<form class="contact-form" action="" method="post">
  <input name="name" type="text" placeholder="Your Name" />
  <input name="mail" type="email" placeholder="Your Email" />
  <textarea name="message" placeholder="Your Message"></textarea>
  <button type="button">Send Message</button>
</form>
```

**Better Way**

The following path is an example of a sending service to build later. It does not exist in your project:

```html
<form class="contact-form" action="/contact" method="post">
  <label for="contact-name">Your name</label>
  <input id="contact-name" name="name" type="text" autocomplete="name" required />

  <label for="contact-email">Your email</label>
  <input id="contact-email" name="email" type="email" autocomplete="email" required />

  <label for="contact-message">Your message</label>
  <textarea id="contact-message" name="message" required></textarea>

  <button type="submit">Send Message</button>
</form>
```

**Note**

I tested clicking `Send Message`. Nothing was sent, and the page did not navigate. The direct cause is `type="button"` with no JavaScript. `type="submit"` asks to send the form, but it does not create a server to receive the data. `action=""` sends to the current page’s address. `action="#"` in Subscribe is not a subscription service.

Using `type="email"` was a good step. But an empty value is allowed unless you add `required`. Add it only to fields that really are required. `name` is the name of the submitted data. `autocomplete` tells the browser what kind of user data belongs in the field, making it easier to fill.

You already answered questions about connecting `label` and `input` in T001. I will not repeat that basic lesson. The gap here is applying what you learned to the real fields. `04-accessibility.md` explains the effect of missing labels and focus styles. Adding labels inside a flex form affects spacing. Review how each label and its field are grouped when you implement this.

## [4] Landing and Skills: heading level does not choose font size

▲ Better Approach — the heading structure needs adjustment

**Your Code**

```html
<h2>
  HELLO WORLD!<br />
  WE ARE KASPER,WE MAKE ART.
</h2>
```

And in Skills:

```html
<h3>Testimonials</h3>
```

```html
<h3>Our Skills</h3>
```

**Better Way**

```html
<h1>
  HELLO WORLD!<br />
  WE ARE KASPER, WE MAKE ART.
</h1>
```

Use `h2` for Testimonials and Our Skills if they remain two main topics at the same level. Or add a shared `h2` above them and keep them as `h3`. You do not need both options.

**Note**

The page has no `h1`. This does not stop rendering, but it leaves the page without a clear main heading. Most sections follow `h2 → h3` well. The exception is Skills. Its headings are at the level used for items within sections, even though they introduce main topics.

Changing the element means updating `.landing .text .content h2` to target `h1`, and updating the matching Skills heading rules. Keep the intended appearance. Do not choose `h3` because its default size is smaller. CSS controls size. You also do not need to turn every `div` into a `section` or add a visible heading to every decorative strip.

## [5] Stats and Skills: a heading or a measured value?

💡 New Tool / Concept

**Your Code**

```html
<h3>1,263</h3>
<p>Coffee Drinks</p>
```

```html
<div class="prog-holder">
  <h3>Adope</h3>
  <div class="prog">
    <span style="width: 90%" data-progress="90%"></span>
  </div>
</div>
```

**Better Way**

The simplest change for a statistic: the number is visually highlighted text, not a heading:

```html
<p class="stat-value">1,263</p>
<p>Coffee Drinks</p>
```

If the Skills percentage is a real measurement on a known scale:

```html
<label for="adobe-skill">Adobe</label>
<meter id="adobe-skill" min="0" max="100" value="90">90%</meter>
```

**Note**

`meter` represents a value within a range. `progress` represents progress through a task, such as uploading a file. The CSS name `.prog` does not give a group of `div/span` elements the meaning of a measurement. [MDN: meter](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meter).

This is a semantic choice. It does not promise that the default appearance will match your design. Its styling needs testing across browsers. A written description of experience may also be more useful than an arbitrary percentage in a personal portfolio. The current numbers do not prove your actual JavaScript or PHP skills.

You do not need to rebuild all of Stats. But `dl` with `dt/dd` is a later option for several name/value pairs. Decorative icons can stay inside a wrapper that creates the circle. That is not an unnecessary wrapper.

# New Things Worth Learning From This Project

1. **Interactive content**: choose one control based on behavior, because of the `button > a` problem.
2. **Fragment links**: connect `href` to `id` to complete your page navigation without JavaScript.
3. **Native form submission**: separate submit, validation, and the service that receives the data.
4. **meter and dl**: give values and measurements meaning instead of using headings just to enlarge numbers.
