OmniGraffle SVG 7.5 exports SVG that occasionally has text formatting glitches (yes its creators know about the problem).

# Example of broken SVG inside HTML

The problems are highlighted in yellow in this screenshot.

![image](https://user-images.githubusercontent.com/82182/34155432-43abf2d2-e487-11e7-8de1-576a8245dc9d.png)

^ [example_without_javascript.html](/paul-hammant/OmniGraffle_7.5_SVG_fixer/blob/master/example_without_javascript.html)

# This JavaScript fixes that..

.. but only when the SVG is inserted into HTML for display inline:

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
<script>
// JavaScript magic here to fix the above tspan elements
$('text', 'svg').each(function() {
    var TEXT_LENGTH = 'textLength',
        $prev, prevLength = 0, prevY,
        $this, thisLength = 0, thisY,
        sameCounter = 1;

    $(this).children('tspan').each(function() {
        $this = $(this),
        thisY = $this.attr('y'),
        thisLength = parseFloat($this.attr(TEXT_LENGTH));

        if (prevY === thisY) {
            sameCounter++;
            prevLength += thisLength;
            $prev.attr(TEXT_LENGTH, prevLength);
            $prev.text($prev.text() + $this.text());
            $this.remove();
        } else {
            logResult(sameCounter, thisY);
            $prev = $this;
            prevY = thisY;
            sameCounter = 1;
            prevLength = thisLength;
        }
    });

    logResult(sameCounter, thisY);
});

function logResult(number, valueY) {
    if (number > 1) {
       console.log('Combine ' + number + ' <tspan> blocks with the same y = ' + valueY);
    }
}
</script>
```

# The same example fixed!

![image](https://user-images.githubusercontent.com/82182/34155543-c13c561a-e487-11e7-8315-e4e5e713871c.png)

^ [example_WITH_javascript.html](/paul-hammant/OmniGraffle_7.5_SVG_fixer/blob/master/example_WITH_javascript.html)

# Credits

The JavaScript was written by quickly written by the excellent [Gleb Kemarsky](http://glebkema.ru/), and
is MIT licensed so you can reuse it on your site.
