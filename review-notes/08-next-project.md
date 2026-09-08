# Lessons For My Next Project

Review date: **2026-09-08**. I reviewed the HTML, CSS, and local assets. I ran the page and checked screenshots, layout, and keyboard behavior. I did not change the site. I created only the eight review files.

**Summary:** You have a practical base for building a page with many sections. You do not need to restart HTML/CSS. But the current version is not yet a responsive site with complete interaction and accessibility. The main gap is not how many properties you remember. It is knowing **what a property measures, which context it works in, and how to prove that the result is correct**.

Start with this file. Then work on one site section per session. Read `03-responsive.md` for layout issues, then the related note in the other files. Do not try to apply every suggestion in one cleanup session.

## 1. Don't Repeat These

- Do not set a fixed minimum larger than the container and expect Grid to shrink it. Portfolio is the direct example.
- Do not use `absolute` just to align text to the right or place price parts next to each other.
- Do not test only far from breakpoints. Check the exact value and just before and after it. The 768 case showed two different bugs.
- Do not present `href="#"`, hover, and cursor pointer as completed features.
- Do not remove a focus indicator just because it does not match the design. Do not choose secondary text colors by eye alone.
- Do not write a selector from memory and add `!important` before checking that the element exists and the rule matches.

## 2. Keep Doing These

- The page structure is clear. Sections are grouped in HTML and CSS. `main`, `nav`, `blockquote`, and `address` are suitable choices in their locations.
- `.container` and `.main-heading` reuse real design choices. They are not unnecessary abstractions.
- You used Flexbox, Grid, and `gap` to arrange content instead of text spaces or `<br>` to create columns.
- The current Services implementation separates element direction from text alignment on phones.
- The current Pricing implementation calculates widths with gaps and changes to 1/2/4 columns. Its links have focus-visible, and the transition names its properties. These are strengths of **the current result**.
- Subscribe uses stretch to fill the button’s height instead of relying on a fixed height.
- You have color and spacing variables, a small WebP background, and working local asset paths. You did not need a framework to build a full page.

A fair note: some of Pricing received help earlier in the conversation. I can praise the result, but I cannot use that part alone to prove that you can redesign it or debug its calculations independently.

## 3. Things I Still Seem Unsure About

These are possible gaps suggested by your code choices. They need confirmation from you. They are not certain judgments about what you understand:

| Likely gap | Current evidence | Needed idea |
| --- | --- | --- |
| Aligning a box versus its text lines | Your questions about Services and fit-content, with different solutions in the code | Choose the layout context, axis, and responsible element |
| References for percentages and limits | Pricing currency is tied to p width; Grid starts at 400px | Relative to which box? What is the smallest allowed space? |
| Normal flow | The testimonial name and Video caption are absolute | Does the parent count this content’s space? |
| Cascade and matching | !important in Design and a Subscribe selector for a missing element | Check matching, then specificity, then whether the property fits |
| Appearance versus semantics and behavior | A button inside a link, li filters, typebutton send buttons | Choose the control, its function, and its states before styling it |
| Finishing a component professionally | Hidden focus, missing labels, weak contrast | A complete component supports use, not just a screenshot |

Connecting `label/for/id` is not unknown to you. T001 records correct answers and an implementation after a request to complete the code. Fields without labels here show that the project needs that knowledge applied. **They do not prove that you failed the lesson later.** The current application files come from before that test.

## 4. New Things Worth Learning Next

For now, stay with topics that directly extend your real problems:

1. `object-fit` and `object-position` to style images without distortion.
2. Intrinsic/content sizing and flexible Grid limits, instead of treating every width with a new value.
3. Native controls, form submission, and accessible names. Your interfaces need these parts.
4. DevTools: box model, computed styles, matched rules, and overflow measurements. The tool helps explain CSS instead of trying random properties.
5. When moving to JavaScript, learn state and events through a menu and filter in **this project**. Do not add libraries before understanding events, state, and DOM updates.

You do not need React, Vue, Redux, or an AI course now to fix these gaps. Using AI for review and explanations is fine. But a copied result only becomes evidence of independence when you can explain, change, and check it. This is not an assessment of your past knowledge of those tools. They were not tested here.

## 5. Mistakes I Repeated

These patterns repeated within this project:

- **Incomplete visual controls:** Header, Landing, Portfolio, Pricing, Forms, and Footer.
- **Missing support for use:** labels in both forms, removed outlines in both, and weak colors in several sections.
- **Positioning instead of a layout relationship:** price parts, the testimonial author’s name, and video text. The causes are related, but the effects differ.
- **Image dimensions without a fit choice:** Portfolio images and the first Testimonials image.
- **Styling tied to details that should not control it:** long selectors, and active tied to order instead of state.

An earlier review, R001, is available for this same project, dated 2026-09-06. It is not a documented review of your first project. Some findings overlap. But the current application has not changed from the source discussed then. **I am not recording these as mistakes you made again in a new project or after a successful fix.** What is new in this review is visual checking, measurements, and keyboard tests. Rendering was unavailable in the earlier review.

The Grid article A001 was not accepted earlier because of its presentation. Mentioning Grid in these files does not record a passed test or remove the restriction against testing A001. This is review material provided to you, not proof that learning has been gained.

## 6. Before Starting My Next Project

A short checklist to apply to your current project first:

- [ ] Choose the correct font and identity before setting final sizes.
- [ ] Fix Portfolio containment and Header/Stats boundary coverage. Then measure overflow again.
- [ ] Give Video and testimonials enough room to read well on phones.
- [ ] Connect navigation to real sections and choose interactive elements without nesting controls.
- [ ] Apply labels, focus styles, and readable text colors. Try the page once without a mouse.
- [ ] State honestly what is static and what works, especially forms, search, and buying.
- [ ] Change one idea at a time. Check the result, then review git diff before a commit.

**Section-by-section review order and a goal you can check:**

| Section | Current state and work priority | Goal before moving on |
| --- | --- | --- |
| Global / Header | Font mismatch; all links use #; menu is hidden at 768 and hover-only | Correct font, real destinations, usable menu |
| Landing | Consistent desktop appearance; arrows and dots are static; positioning is weak at short heights | Space for text and header; clear notice of unimplemented interaction |
| Services | Good layout and clear overall appearance; weak text contrast | Keep the layout and improve reading |
| Design | Decorative crop looks intentional; !important is used because of specificity | Remove the local conflict, without an unnecessary redesign |
| Portfolio | Overflow, distorted images, visual-only filters, and button>link | Images fit, correct ratio, proper controls/links |
| Video | Caption hides controls on phones | All information and controls available without overlap |
| About | Image crop is consistent at checked sizes | Keep it and check after font/spacing changes |
| Stats | 768 boundary issue; numbers used as headings | Stable layout and clearer meaning for values |
| Testimonials / Skills | Cramped text, distorted image, and absolute author name | Comfortable text, correct image, name in flow |
| Quote | Suitable blockquote and author credit; no clear layout bug | Keep the structure; do not invent a refactor |
| Pricing | Strong layout; currency and unit separate visually as the card widens; buying is not connected | Price parts grouped together and an honest CTA destination |
| Subscribe | Good stretch; unused selector, missing label/focus, and incomplete sending | Named, readable field and a defined function |
| Contact | Suitable layout; labels/focus/contrast and sending are incomplete | Clear form experience, without claiming a backend exists |
| Footer | Organized; icons are not links and the dark logo is hard to see | Real links when available; check identity and text |

After this functional pass, read `06-clean-code.md` and do a separate small cleanup. For performance, start with the video, then images and fonts, not micro-optimizations.

## 7. My 5 Highest-Priority Lessons

1. **Diagnose before changing:** which element, rule, axis, and containing block? This prevents a chain of random fixes.
2. **Let content show when change is needed:** make minimum sizes fit their containers. Test width, height, and media query boundaries.
3. **Normal flow is the starting point for content:** use positioning when you really need an overlay relationship, not just alignment.
4. **A complete interface is usable:** the right element, destination or event, label, focus, and contrast are all part of quality.
5. **Check your own work:** a good desktop screenshot does not prove mobile support, keyboard use, or independence. Reproduce the problem, then prove that its cause is gone.

## 8. Readiness Assessment

This assessment uses the available evidence, not your years of study or how long you worked. **Conceptual Understanding** means your ability to explain the cause. **Practical Application** means the quality of the current visible implementation. **Debugging Ability** means your ability to find the fault, not my ability to diagnose it.

| Area | Conceptual Understanding | Practical Application | Debugging Ability | Evidence and limits |
| --- | --- | --- | --- | --- |
| HTML | Untested in this review | Developing | Untested | Organized structure, with nested controls and placeholder links |
| CSS | Untested | Developing | Untested | Clear layout systems and components, with fit/position/cascade gaps |
| Responsive Design | Untested | Developing | Untested | Some successful rearrangement; confirmed overflow, 768 boundary issues, and video overlap |
| Accessibility | Untested overall | Weak in the current version | Untested overall | Menu, focus, contrast, and labels; earlier for/id success has a narrow scope |
| SEO basics | Untested | Developing | Untested | Metadata exists, but the name is inconsistent and the content is from a template |
| Semantic HTML | Untested | Developing | Untested | Good landmarks; heading levels and control meanings need adjustment |
| Clean Code | Untested | Developing | Untested | Good organization and reuse, with deep selectors and unused rules |
| Debugging | Untested | Untested as an independent process | Untested | I produced the bug report; you did not perform a debugging session |
| Design-to-code | Untested | Developing for visual consistency; matching a reference is Untested | Untested | Some visual consistency, but no design reference to compare against |
| Independence | Untested | Untested | Untested | No new task completed in front of me without help; Pricing includes earlier help |

These are not new exam grades. They do not lower the confirmed T001 results. The specific evidence for connecting label and input stays **Developing / Developing / Developing**, as recorded earlier. We do not apply that rating to all of accessibility.

**My decision: move to JavaScript while continuing HTML/CSS through projects, after a focused round of fixes for these basic issues.** Do not wait to “master all of CSS.” But do not skip overflow, focus, and semantics as if they were decoration. Use this page to learn menu state or one filter after correcting its HTML.

Can I prove that you can independently build and debug a reasonably professional site now? **Not yet, based on the current evidence.** I see enough practical ability to move forward and improve. I see no reason to send you back to the start. Later, independence should be shown through a new component, explanations of your choices, and checking the result. Reading or copying Better Way is not enough. I cannot set a job date or estimate hours from this artifact.

### Delivery and record limits

The suggestions were not applied or tested as a corrected version. I did not test multiple browsers, a real screen reader, a backend, or a pixel-perfect match to a design reference. The review files themselves record the dated findings and evidence.

To follow your latest request to “create only the eight review files,” I did not edit the shared `Learning-State.md` or any existing file. The summary not copied into that record is: **a final local review with visual and keyboard checks; learning material delivered without a new test; T001 and the A001 restriction remain unchanged.**
