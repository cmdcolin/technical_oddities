# technical_oddities

A compilation of short descriptions of tech oddities, debunked or not

## Chrome - Adding many strings together causes a crash

If you add a very large number of strings together in javascript, chrome will crash with a call stack error, but it will be very hard to find what part of the code (e.g. if this was in a larger project) generated this error because breakpoints near this error fail in DevTools

Try running the following and opening it up in your browser

    (echo "<script>var s='Generate a call stack error'";for i in {1..100000}; do echo "+' $i'"; done; echo "</script>")>err.html

## Javascript - Maximum size of a HTML5 canvas element

Many browsers have a maximum size of an HTML5 canvas element, typically with any one dimension limited to the max size of a 16 bit signed int http://stackoverflow.com/questions/6081483/maximum-size-of-a-canvas-element

Of course, creating one larger than this throws no errors neither from the creation nor the drawing to it (no drawing actions will complete)


## PhantomJS - The size of the default form elements don't scale with zoomFactor

Using zoomFactor is a promising way to make a website render "retina" type screenshots from webpages, but using a higher zoomFactor will scale all web page elements except the size of checkboxes, radio elements, and other form elements.

Source code a screenshot demonstrating issue here https://gist.github.com/cmdcolin/05a916eead041dd4288cd461b15b0197

When you dig into it, it seems to come back to a WebKit core issues that are confusing to follow https://bugs.webkit.org/show_bug.cgi?id=51544


