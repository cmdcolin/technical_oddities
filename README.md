# technical_oddities

A compilation of short descriptions of tech oddities, debunked or not

## Chrome - Adding many strings together causes a crash

If you add a very large number of strings together in javascript, chrome will crash with a call stack error, but it will be very hard to find what part of the code (e.g. if this was in a larger project) generated this error because breakpoints near this error fail in DevTools

Try running the following and opening it up in your browser

    (echo "<script>var s='Generate a call stack error'";for i in {1..100000}; do echo "+' $i'"; done; echo "</script>")>err.html

edit 2023: may not crash chrome anymore, even if increases to 1,000,000, but may have similar type of situation with many ternary https://twitter.com/cmdcolin/status/1640104370800066561

## Javascript - Maximum size of a HTML5 canvas element

Many browsers have a maximum size of an HTML5 canvas element, typically with any one dimension limited to the max size of a 16 bit signed int http://stackoverflow.com/questions/6081483/maximum-size-of-a-canvas-element but considerably less when considering both dimensions

A quite jarring consequence though is that creating a canvas larger than allowed limits does not throw an error from the creation or the drawing to it

See also https://github.com/jhildenbiddle/canvas-size#test-results

## PhantomJS - The size of the default form elements don't scale with zoomFactor

Using zoomFactor is a promising way to make a website render "retina" type screenshots from webpages, but using a higher zoomFactor will scale all web page elements except the size of checkboxes, radio elements, and other form elements.

Source code a screenshot demonstrating issue here https://gist.github.com/cmdcolin/05a916eead041dd4288cd461b15b0197

When you dig into it, it seems to come back to a WebKit core issues that are confusing to follow https://bugs.webkit.org/show_bug.cgi?id=51544

update 2023: try in puppeteer...will report back

## SVG - large feature sizes are not drawn

Example

```
  <svg width="1400px" height="8px">
    <rect
      data-testid="bb-535451550-0"
      x="-100000000"
      y="0"
      width="100000100"
      height="3.3333333333333335"
      fill="goldenrod"
    ></rect>
  </svg>

```

In Chrome 83 this is not visible on the screen, but ideally would be a 100px wide box

## Canvas - large clearRect does not clear region properly

Similar to the above SVG issue, performing something similar, clearing a large region from offscreen areas, with clearRect also fails on canvas

```
// this fails and the red bar disappears
const elt = document.getElementById("cnv"); // canvas with width=1000 height=10
const ctx = elt.getContext("2d");
ctx.fillStyle = "#f00a";
ctx.fillRect(0, 0, 1000, 10, 10);
ctx.clearRect(-94504.1, 0, 29790.800000000003, 7);

// this works and the green bar does not disappear
const elt2 = document.getElementById("cnv2"); // canvas with width=1000 height=100
const ctx2 = elt2.getContext("2d");
ctx2.fillStyle = "#0f0a";
ctx2.fillRect(0, 0, 1000, 10, 10);
ctx2.clearRect(-94504.1, 0, 29790.800000000003, 7);
```

See 

https://bugs.chromium.org/p/chromium/issues/detail?id=1131528#c_ts1602745683
https://codesandbox.io/s/sleepy-fermi-lhy2f?file=/src/index.js

## Can only store 2^24 elements in Map or ~2^23 in regular JS object

See https://searchvoidstar.tumblr.com/post/659634228574715904/an-amazing-error-message-if-you-put-more-than-2-24

Only affects some platforms e.g. V8


### Chrome - The maximum string length is 512MB. If you use TextDecoder it returns silently if exceeded

See https://cmdcolin.github.io/2021-10-30-spooky.html
