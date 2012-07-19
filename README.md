# 1.

**Setup:**
 - I don't really get this question. It could be a fat client architecture, where the server just exposes a JSON API, and client takes care of view rendering, it could be pure server side view rendering, or it could be an interim solution(which probably I would choose), where the first load of the page is rendered by the server, but subsequent ajax requests only return partial JSON or HTML that the client should process and insert on its own.

**Compatibility:**
 - The number of pages should not be relevant to the browser compatibility. There should be automated (e.g. Selenium) tests that test for every possible user interaction, with every allowed input values, in every supported browser. On the visual side, most probably manual testing is needed to verify proper rendering in different browsers.

# 2.

**Potential bottlenecks:**
 - Too many parallel requests towards an HTTP endpoint (browsers only open 2-6 concurrent TCP sockets towards an endpoint -> combine, minify resources..)
 - Network bandwidth (For upload, cookies can grow requests -> use different domain for cookie-free resources) (For download -> enable HTTP gzip compression)
 - Including scripts and styles in <head> (fetching, parsing) is done synchronously, so it effectively delays the page parsing and rendering (-> use deferred and async scripts..)
 - Computing intensive javascript included or embedded anywhere on the page delays further parsing and rendering (UI and javascript are usually running on the same thread).

**Improvements:**
 - I am not familiar with the website, but after a quick check, one thing I would improve is that, a lot of user actions trigger full page reload, instead of doing it the ajax way, which would result in faster load times and less flickering.
 - + Pro tip: your images can be further (lossless) optimized (for example I could chop off 20% from [this sprite](http://cache4.hyves-static.net/images/redesign/buttons/buttons_sprite.956b41c6.png).)

# 3.

**The used technique is:**
 - The opaque content is wrapped inside a container div, and a new element responsible for the transparent background is inserted as well into the container. The background div is removed from the flow with absolute positioning, stretched to the size of the container, has background-color set, made transparent with opacity(filter for &lt;=IE8) property, and has negative z-index set not to overlap the content itself.

**Improvement for this technique:**
 - Simplify markup by generating the background element from CSS, for example with the :before pseudo class. (http://caniuse.com/#search=pseudo)

```css
  .box-con-inner.transparent:before {
    content: '';
    /* ... existing definition from bg class */
  }
```

**Other possible solutions:**
 - Using an alpha-transparent png as container background (http://caniuse.com/#search=png%20alpha)
 - Using rgba/hsla background color for container element with alpha value < 1 (http://caniuse.com/#search=rgba)
 - Other extremities: use svg, with alpha filter or fill/stroke-opacity (http://caniuse.com/#search=svg); insert canvas, use rgba fillStyle (http://caniuse.com/#search=canvas)

# 4.

**One button:**
 - Wrap the button in an anchor element, so a window will still open if javascript is disabled (graceful degradation). Simply bind click handler to anchor.

**Many buttons:**
 - Make use of event bubbling and bind event handler to a parent element containing all the thousand buttons, and check event.target to find out if it came from an anchor, and which one. (See performance considerations at http://api.jquery.com/on/#direct-and-delegated-events)

Please find example implementation with jQuery [here](http://atimb.github.com/mustached-tyrion/example.html).

# 5.

Please find CSS declarations with example rendering [here](http://atimb.github.com/mustached-tyrion/example.html).

Could be also done with something like: (but this CSS3 selector is not compatible with IE<9)

```css
li {
  float: left;
}
li:nth-child(4n+1) {
  clear: left;
}
```