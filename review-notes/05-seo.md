# SEO — The basics that fit a single page

This is a review of local content and structure, not a Google indexing or ranking report. I did not test the published version or Search Console. You have `lang="en"`, charset, viewport, title, and description. The content is directly in HTML. The starting structure is already there.

## [1] The page title and description use another project’s name

❌ Problem

**Your Code**

```html
<meta
  name="description"
  content="Leon is a creative agency specializing in web design, graphic design, and digital services."
/>
<title>Leon | Creative Design Agency</title>
```

**Better Way**

The visible content says Kasper. This is also an exercise, not a confirmed agency:

```html
<title>Kasper | Responsive HTML &amp; CSS Practice Project</title>
<meta
  name="description"
  content="A responsive HTML and CSS practice project featuring service, portfolio, pricing, and contact sections."
/>
```

**Note**

The problem is not missing metadata. It is metadata that does not match the page. Changing the image file or hero does not update `<title>` automatically. Write an honest description of the published version’s purpose. If it becomes a real agency site, update the description based on its real services.

The title helps users and search engines understand the page. But it does not guarantee that Google will always show the exact text. Google may build a title link from other signals. [Google Search Central: Title links](https://developers.google.com/search/docs/appearance/title-link).

## [2] Links, headings, and real content matter more than adding many tags

▲ Better Approach

**Your Code**

```html
<h3>Awesome Image</h3>
<p>Photography</p>
```

```html
<a href="#" class="pricing-link">Buy Now</a>
```

**Better Way**

For example, for the work shown in the first image:

```html
<h3>Mountain Lake Photography</h3>
<p>Landscape photography study</p>
```

Replace the purchase link’s destination with a correct one **when it exists**, not an invented address. If the interface is a demo, state that buying is not available.

**Note**

Repeating a general name and filler text is fine while practicing layout. But it does not explain your work when you put the project in a portfolio for job applications. A clear name and honest description serve the user first. Do not claim image ownership or business experience just because a template shows it.

The missing `h1`, `#` links, and work-image alt text are covered in `01-html.md` and `04-accessibility.md`. You do not need three explanations of the same issue under SEO, HTML, and accessibility. Semantic HTML does not guarantee rankings. Adding more `section` elements is not an SEO strategy.

## [3] Publishing improvements that are not urgent

▲ Better Approach

**Your Code**

```html
<html lang="en">
```

**Better Way**

Keep this while English is the main language of the content. I found no reason to add agency or commercial pricing structured data to this practice version.

**Note**

The browser requested `/favicon.ico`, and the server returned 404. The page assets themselves loaded successfully. A favicon is useful later for tab identity. But this is not a reason to call all rendering or SEO broken.

After choosing the final published URL, you can consider a sharing preview. Do not add a canonical link to a guessed URL or a robots rule that blocks your project by mistake. For a single-page site, a sitemap is not a priority before fixing links, content, and the title.

# New Things Worth Learning From This Project

1. **Metadata consistency**: check the title and description when moving a template between projects.
2. **Descriptive content**: use a real project name and description instead of keywords or repeated general text.
