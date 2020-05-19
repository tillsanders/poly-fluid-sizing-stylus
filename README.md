# Poly Fluid Sizing for Stylus

> Stylus mixin for linear interpolation between multiple values across multiple breakpoints using
> CSS calc() and viewport units. Adaption of the SASS mixin by
> [Jakobud/poly-fluid-sizing](https://github.com/Jakobud/poly-fluid-sizing).

## Usage

Poly Fluid Sizing is a [stylus](https://stylus-lang.com/) mixin that gives you precise control over CSS measurements, like the `font-size` across multiple breakpoints. It allows you to set the desired scale for every breakpoint and applies linear interpolation between them using `calc()`. It uses some ~~basic~~ dark math behind the scenes. You don't need to know the math or understand it to use this mixin.

```stylus
// Import mixin
@require 'poly-fluid-sizing'

h1
  poly-fluid-sizing('font-size', {'30rem': 2rem, '50rem': 3rem, '60rem': 4rem})
```

This will yield the following CSS code (without the comments):

```css
h1 {
  /* The minimum font-size */
  font-size: 2rem;
}
@media (min-width: 30rem) {
  h1 {
    /* Interpolating the font-size between 2rem @ 30rem and 3rem @ 50rem viewport width */
    font-size: calc(5vw + 0.5rem);
  }
}
@media (min-width: 50rem) {
  h1 {
    /* Interpolating the font-size between 3rem @ 50rem and 4rem @ 60rem viewport width */
    font-size: calc(10vw - 2rem);
  }
}
@media (min-width: 60rem) {
  /* The maximum font-size */
  h1 {
    font-size: 4rem;
  }
}
```

## It can do more then `font-size`

Using Poly Fluid Sizing on the `font-size` property is an obvious use case. It can however be used
for any other numeric CSS property:

```stylus
main
  poly-fluid-sizing('padding', {'30rem': 1rem, '50rem': 3rem})
```

## Why not CSS min/max/clamp?

The vanilla CSS functions `min()`, `max()` and `clamp()` can be used to achieve a very similar
effect to that of this mixin. If you want to know how, I recommend this
[article by CSS-Tricks.com](https://css-tricks.com/min-max-and-clamp-are-css-magic/).

### Advantages over vanilla CSS

- **Browser Support:** IE/Edge currently (Edge v18) still doesn`t support min/max/clamp. Firefox >
  v74, Chrome > v78. See here: https://caniuse.com/#feat=mdn-css_types_min
- **Multiple breakpoints:** This mixin is able to support multiple breakpoints. Although, in most
  cases, you could probably do without that.
- **Easy to configure:** The vanilla CSS solution using `clamp()` is not to hard to use, but the
  syntax of this mixin is a little easier to parse for our brains.

### Disadvantages compared to vanilla CSS and limitations

- **Verbosity**: This mixin generates more code compared to vanilla CSS.
- **You can't mix units**: Because the interpolation is calculated at build time, you can use
  various units like px or rem, but you can't mix them. For example, you can't set a `2em`
  `font-size` for `1000px` viewport width. This is a logical and mathematical limitation.
- **One property at a time**: You can choose any numeric CSS property that you like, but you can
  only ever use one at a time. That means, you can use `padding`, but if you only want the sizing to
  be applied to `padding-left` and `padding-right`, you have to call the mixin twice.

## Credits

This mixin is a stylus adaption of the original, wonderful SASS mixin by
[https://github.com/Jakobud](Jakobud). You can learn more about this technique from the author
himself on [Smashing Magazine](https://www.smashingmagazine.com/2017/05/fluid-responsive-typography-css-poly-fluid-sizing/).