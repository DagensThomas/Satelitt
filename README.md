# Orbit, 2006 to 2026: scroll-driven satellite visual

A single self-contained page (`index.html`, no build step, no dependencies except one Google Font).
A blue globe stays still on screen while a count of active satellites grows with the reader's scroll:
813 (2006), 1 419 (2016), 16 914 (2026).

## Publish on GitHub Pages

1. Create a repository and put `index.html` in the root.
2. Repository **Settings -> Pages**: deploy from the `main` branch, root folder.
3. After a minute the page is live at `https://<user>.github.io/<repo>/`.

Open the URL to check it. Scrolling the page drives the animation.
GitHub Pages sites are public, so don't put anything unpublished in the repo.

## Use it from another page (iframe mode)

Add `?mode=embed` to the URL:

    https://<user>.github.io/<repo>/?mode=embed

In this mode the page ignores its own scrolling and follows progress sent by the parent page.

**Parent -> page.** Send a number from 0 (empty globe) to 1 (end of 2026):

    iframe.contentWindow.postMessage({ type: 'orbit-progress', value: 0.42 }, '*');

The page only accepts these messages from its parent window.

**Page -> parent.** Whenever the year or count changes the page posts:

    { type: 'orbit-state', year: '2016', count: 1419, stage: 1 }

`year` is `null` before the first stage. Use this to show the year and number in your own text
elements and font instead of the small built-in label.

## Important: a plain iframe cannot see the parent's scroll

An iframe on its own does not know how far the reader has scrolled in the parent page.
Something on the parent side has to measure the scroll and send `orbit-progress` messages,
for example a small custom Vev component (React) that wraps the iframe. That bridge is not
included here and has not been tested in Vev or Dr. Publish.

## Data and caveats

- 2006: 813 active satellites (UCS Satellite Database, 11 Feb 2006).
- 2016: 1 419 (UCS update, June 2016).
- 2026: 16 914 (Jonathan McDowell's count, 30 Sept 2026).
- UCS and McDowell define "active" slightly differently (McDowell's figures run about 5% higher), so the
  2006 and 2016 comparison with 2026 is approximate.
- Satellite positions and motion are illustrative. Only the counts are data.
- The continents are hand-drawn and coarse. Replace them with Natural Earth (public domain) before publishing.
- The font is Schibsted Grotesk from Google Fonts. Swap the `--sans` variable for your brand font.
- The last stage draws about 17 000 squares every frame. Check it on a mid-range phone.

## Timings

The stages are defined at the top of the script in the `T` array, in "timeline units"
(`s` = start, `d` = duration, `TOTAL` = full length). The page is 520vh tall, so one unit is about 28vh of scrolling.
Change `#track{height:520vh}` in the CSS to make the whole sequence longer or shorter.
