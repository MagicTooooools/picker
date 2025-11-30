---
title: Progress Bar Labels with CSS Anchor Positioning
url: https://blog.1a23.com/2025/11/29/progress-bar-labels-with-css-anchor-positioning/
source: 1A23 Studio
date: 2025-11-29
fetch_date: 2025-11-30T03:29:01.149564
---

# Progress Bar Labels with CSS Anchor Positioning

[![1A23 Blog](https://blog.1a23.com/wp-content/uploads/sites/2/2020/12/cropped-Lockup-portrait-blog-foreground-Copy-3.png)](https://blog.1a23.com/)

* [1A23 Studio](https://1a23.com)

# Progress Bar Labels with CSS Anchor Positioning

[2025-11-29](https://blog.1a23.com/2025/11/29/progress-bar-labels-with-css-anchor-positioning/)

—

in [Tech](https://blog.1a23.com/category/tech/)

![](https://blog.1a23.com/wp-content/uploads/sites/2/2025/11/image-png.avif)

Here’s a small but fun use case for CSS Anchor Positioning, using it to keep multiple labels around a progress bar readable, without touching JavaScript.

The goal is simple: we have a horizontal progress bar with three labels: one on the left, one on the right, and one that should follow the progress “tip”. We want that center label to sit as close as possible to the current progress, **but never overlap** with either side label.

To make things easier, we’ll assume:

* Only the center label needs to move dynamically, the side labels stay at their ends.
* The progress bar is wide enough to fit all three labels with some spacing.

CodePen Embed Fallback

## Describing the layout as rules

Instead of hard-coding coordinates, it helps to think in terms of a small set of layout rules the browser can try in order.

For the center label, the logic can be written as:

1. Try placing the center label **to the left** of the progress tip, with a margin.
2. If that would overlap something, try placing it **to the right** of the progress tip, also with a margin.
3. If the tip is already too close to another label, place the center label **next to that label instead**, with a wider margin to keep everything readable.

CSS Anchor Positioning is very good at this style of “try a few positions until one fits” layout. The `@position-try` at-rules let us declare those strategies explicitly and let the browser pick whichever one does not collide with the container or other constraints.

In this example, I use three named anchors:

* `--prog-bar`: the progress bar itself (specifically, its right edge as the “tip”)
* `--left-label`: the fixed label on the left
* `--right-label`: the fixed label on the right.

The moving center label is then positioned relative to these anchors.

## Defining `@position-try` strategies

Here are the two position-try rules that implement the logic above:

```
1

@position-try --bar-left {

2

position-area: none;

3

right: max(

4

calc(anchor(--prog-bar right) + var(--container-padding)),

5

calc(anchor(--right-label left) + var(--label-gap))

6

);

7

margin-left: calc(

8

var(--container-padding) + anchor-size(--left-label width) +

9

var(--label-gap)

10

);

11

}

12

13

@position-try --bar-right {

14

position-area: none;

15

left: max(

16

calc(anchor(--prog-bar right) + var(--container-padding)),

17

calc(anchor(--left-label right) + var(--label-gap))

18

);

19

margin-right: calc(

20

var(--label-gap) + anchor-size(--right-label width) +

21

var(--container-padding)

22

);

23

}
```

Conceptually:

* `--bar-left` corresponds to “try to sit just to the **left** of the progress tip”.
* `--bar-right` corresponds to “otherwise, sit just to the **right** of the progress tip”.

The “third rule” (move next to whichever label is closer) is effectively folded into the `max()` expressions:

* For `right` / `left` we take the **maximum** of:
  + the tip position plus container padding, and
  + the neighbouring label plus a label gap.

This means:

* If there’s plenty of room, the center label hugs the progress tip.
* If the tip is about to collide with a side label, the center label gets pushed away so that it sits beside that label instead, maintaining at least `var(--label-gap)` of space.

## Avoiding overlap with `anchor-size()`

The margins are where the overlap-avoidance really happens.

Take this line as an example:

```
1

margin-left: calc(

2

var(--container-padding) + anchor-size(--left-label width) +

3

var(--label-gap)

4

);
```

Here we:

* Read the **actual width** of the left label via `anchor-size(--left-label width)`.
* Add the container’s padding and a label gap.
* Use that as the center label’s margin, so its text always clears the left label by at least that gap.

In other words, instead of guessing spacing with hard-coded numbers, we attach the spacing directly to the real measured size of the anchors. That’s exactly the kind of thing `anchor-size()` is designed for.

The same idea is used for `margin-right` with the right label.

## Putting it together

Once the `@position-try` rules are defined, the center label simply opts in to them, for example:

```
1

.label.center {

2

position: absolute;

3

top: var(--container-padding);

4

position-try: --bar-left, --bar-right;

5

position-anchor: --prog-bar;

6

position-area: left;

7

}
```

The browser will then:

1. Try `--bar-left`.
2. If that would cause an overlap or violate constraints, fall back to `--bar-right`.

Combined with the `max()` insets and `anchor-size()`-based margins, that’s enough to:

* Keep the center label near the progress tip,
* Prevent it from overlapping either side label,
* Adjust spacing automatically if label text length changes,
* Do all of this with **zero JavaScript**.

The full implementation – including the anchors and layout container – is in the CodePen embed above.

## Browser support

CSS Anchor Positioning is still relatively new, and support is rolling out across browsers. Before using this technique in production, it’s worth checking current support status or adding a progressive enhancement fallback.

You can use the Baseline widget below to see the latest support details for anchor positioning across engines:

[![Baseline Status: Anchor positioning](https://baseline.js.org/features/anchor-positioning/static-adaptive.svg)](https://webstatus.dev/features/anchor-positioning)

Tags:[Anchor Positioning](https://blog.1a23.com/tag/anchor-positioning___en/) [CSS](https://blog.1a23.com/tag/css/) [Web](https://blog.1a23.com/tag/web/)

---

## Comments

## Reposts

* [![](https://s.1a23.studio/files/webpublic-0d33f7ad-778a-4caa-8b23-888a5ea15e4a)](https://s.1a23.studio/%40eana)
  Eana Hufwe

### Leave a Reply [Cancel reply](/2025/11/29/progress-bar-labels-with-css-anchor-positioning/#respond)

Your email address will not be published. Required fields are marked \*

Comment \*

Name \*

Email \*

Website

[ ]  Save my name, email, and website in this browser for the next time I comment.

[ ]  Notify me of follow-up comments by email.

[ ]  Notify me of new posts by email.

Δ

To respond on your own website, enter the URL of your response which should contain a link to this post’s permalink URL. Your response will then appear (possibly after moderation) on this page. Want to update or remove your response? Update or delete your post and re-enter your post’s URL again. ([Find out more about Webmentions.](https://indieweb.org/webmention))

URL/Permalink of your article

←[Previous:  Rotated Background Patterns in CSS with SVG](https://blog.1a23.com/2025/03/06/rotated-background-patterns-in-css-with-svg/)

[![1A23 Blog](https://blog.1a23.com/wp-content/uploads/sites/2/2020/12/cropped-Lockup-portrait-blog-foreground-Copy-3.png)](https://blog.1a23.com/)

信じたものは、都合のいい妄想を繰り返し映し出す鏡。

Designed with [WordPress](https://wordpress.org)

* [1A23 Studio](https://1a23.com)
* [Works](https://1a23.com/works)
* [Lyricova](https://1a23.com/lyricova)
* [Labs](https://labs.1a23.com)
* [Search](https://blog.1a23.com/?page_id=16703)
* [RSS 2.0](https://blog.1a23.com/feed)
* [ActivityPub](https://apfollow.mwt.me/?user=1a23&instance=blog.1a23.com)