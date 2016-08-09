# technical_oddities

A compilation of short descriptions of tech oddities, debunked or not

## Chrome - Adding many strings together causes a crash

If you add a very large number of strings together in javascript, chrome will crash with a call stack error, but it will be very hard to find what part of the code generated this because breakpoints near this error fail in DevTools

Try running the following and opening it up in your browser

    (echo "<script>var s='Generate a call stack error'";for i in {1..100000}; do echo "+' $i'"; done; echo "</script>")>err.html

## Javascript - Maximum size of a HTML5 canvas element

Many browsers have a maximum size of an HTML5 canvas element, typically with any one dimension limited to the max size of a 16 bit signed int http://stackoverflow.com/questions/6081483/maximum-size-of-a-canvas-element

Of course, creating one larger than this throws no errors neither from the creation nor the drawing to it (no drawing actions will complete)



