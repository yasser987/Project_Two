# Performance — The resources that really need your attention

The project is static, with no JavaScript or build dependencies. There are no bundles to reduce. The file sizes below are sizes on disk, not transferred bytes after compression or caching. I did not run Lighthouse, network throttling, or Core Web Vitals tests. I am not giving a performance score or a guessed load time.

All local images, CSS, and webfonts requested by the browser returned 200. The video reached readyState 4. The exception was the automatic favicon request, which returned 404. There were no error/warn entries in the browser log I read. The server log showed the favicon request. I keep those two sources separate.

## [1] Video is the largest resource by far

▲ Better Approach

**Your Code**

```html
<video autoplay loop controls>
  <source src="images/videoSection.mp4" type="video/mp4" />
</video>
```

**Better Way**

If this is a video that users choose to play for its content:

```html
<video controls preload="metadata" playsinline>
  <source src="images/videoSection.mp4" type="video/mp4" />
</video>
```

**Note**

The file is `7,080,771 bytes`, about 7.08MB or 6.75MiB. The Landing background is about 22.6KB. So checking the video choice, compression, and loading policy is more useful than reducing CSS comments.

`preload="metadata"` is a hint to the browser, not a strict promise about data size. Removing autoplay makes playing the video the user’s choice. Do not keep autoplay and expect preload to stop the loading needed for playback. [MDN: video](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/video).

A silent moving background is a different case. It usually needs `muted` and `playsinline` with autoplay, plus a way to stop the movement. Browser policy may block autoplay with sound. The attribute alone does not guarantee playback. The video was not moving automatically in the screenshots I checked. I did not test the cause with the autoplay policy API, so I cannot say that blocking was the only cause. [MDN: Autoplay](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Autoplay).

Choose the video’s role first. The file is not bad just because it is MP4. Compress a version for the needed size and quality after a visual comparison. I do not recommend changing the codec without a compatibility test.

## [2] Images: loading and dimensions matter more than random format changes

▲ Better Approach

**Your Code**

```html
<img src="images/portfolioImageOne.jpg" alt="" />
```

**Better Way**

This example uses the image’s real dimensions while keeping the corrected crop styling in CSS:

```html
<img
  src="images/portfolioImageOne.jpg"
  alt="Snow-covered mountains reflected in a lake"
  width="1500"
  height="1000"
  loading="lazy"
/>
```

**Note**

Portfolio is far below the first screen. `loading="lazy"` suits these images, not the logo or a hero image needed at the start. `width/height` state the image ratio before it loads. They do not force a 1500px display width when CSS sets a responsive width. But they do not fix distortion from `width:100%` and `height:300px` on their own. See object-fit in `02-css.md`.

The first image is `274,740 bytes` at 1500×1000. The other Portfolio images are about 18.8–33.7KB. The first is a clear candidate to compare with a smaller file. But I have no evidence that all your assets are too large. Your logo is about 4.4KB, Phones is about 7.8KB, and Landing is already a small WebP.

Do not claim that a 1500px image is always wrong. High-density screens may benefit from it. When you provide multiple versions later, `srcset/sizes` becomes useful. Do not write paths for versions that do not exist. I did not measure CLS. Some images already have reserved height through CSS. So this is a useful preventive step, not a confirmed performance result.

## [3] Fonts: fix the unused resource before thinking about subsetting

❌ Problem with the font connection; ▲ Better Approach for reducing requests

**Your Code**

```html
href="https://fonts.googleapis.com/css2?family=Work+Sans:ital,wght@0,100..900;1,100..900&display=swap"
```

```css
font-family: "Open Snas", sans-serif;
```

**Better Way**

Either use Work Sans as shown in `02-css.md`, or choose a system font and remove the related Google Fonts links if they are no longer needed. After choosing the font, request the styles and weights the design needs.

**Note**

The confirmed problem is a mismatch, not the loading of 900 font files. The request uses a range for a variable font. Even when an external stylesheet is linked, the browser may not download unused font files. I did not measure Google Fonts transfer here, so I do not count guessed sizes as network traffic.

You already have `preconnect` and `display=swap`. I am not adding them as missing lessons. Font Awesome actually requested the brands and solid files, which returned 200. Having regular and v4compat on disk does not mean they loaded in this session. Choosing between the full library and selected SVG icons is a later trade-off. It is not a reason to manually edit minified vendor files now.

# New Things Worth Learning From This Project

1. **Media loading policy**: when to request metadata and when to play immediately, because the video is the heaviest asset here.
2. **Image dimensions and lazy loading**: reserve the image ratio and delay resources below the first screen.
3. **Network evidence**: separate a file on disk, a requested stylesheet, and a font that was actually used and transferred.
