# 1.

# 2.

# 3.

**The used technique is:**
 - The opaque content is wrapped inside a container div, and a new element responsible for the transparent background is inserted as well into the container. The background div is removed from the flow with absolute positioning, stretched to the size of the container, has background-color set, made transparent with opacity(filter for &lt;=IE8) property, and has negative z-index set not to overlap the content itself.

**Improvements for this solution:**
 - Simplify markup by generating the background element from CSS, for example with the :before pseudo class. (http://caniuse.com/#search=pseudo)

```css
  .box-con-inner.transparent:before {
    content: '';
    /* ... existing definition from bg class */
  }
```

**Other possible solutions:**
 - Using an alpha-transparent png as container background (http://caniuse.com/#search=png%20alpha)
 - Using rgba/hsla background color for container element with alpha value != 1 (http://caniuse.com/#search=rgba)
 - Other extremities: use svg, with alpha filter or fill/stroke-opacity (http://caniuse.com/#search=svg); insert canvas, use rgba fillStyle (http://caniuse.com/#search=canvas)

# 4.

**One button:**
 - Wrap the button in an anchor element, so a window will still open if javascript is disabled (graceful degradation). Simply bind click handler to anchor.

**Many buttons:**
 - Make use of event bubbling and bind event handler to a parent element containing all the thousand buttons, and check event.target to find out if it came from an anchor, and which one. (See performance considerations at http://api.jquery.com/on/#direct-and-delegated-events)

Please find example implementation with jQuery in index.html.

# 5.

Please find CSS declarations with example rendering in index.html.

Could be also done with something like: (but this CSS3 selector is not compatible with IE<9)

```css
li {
  float: left;
}
li:nth-child(4n+1) {
  clear: left;
}
```