Img-bfs is a JavaScript library for <a href="http://en.wikipedia.org/wiki/Breadth-first_search" target="_blank">breadth-first traversal</a> in image.

## API

### Main function

The main function of the library is 

<b>bfs</b>(&lt;HTMLImageElement | HTMLCanvasElement&gt; <i>img</i>, &lt;Coord | Array&gt; <i>startCoord</i>, &lt;Object&gt; <i>eventMap</i>, &lt;Array&gt; <i>blacklist</i>);

It starts search in <i>img</i> image from <i>startCoord</i> coordinate(s) and skips all pixels from <i>blacklist</i>.

### Events

You can subscribe to events via <i>eventMap</i> param. For example:

    bfs(img, {x: 10, y: 10}, {
        onvisit: function(e) {
            console.log(e);
        }
    });

Currently just <i>onvisit</i> event supported. This event rises when img-bfs visits each pixel.

Event object structure:

    {
        pixel: {
            coord: {x: Number, y: Number},
            color: [Number, Number, Number, Number]
        },
        stop: Function,
        skip: Function
    }

Property <i>pixel</i> contains information about visited pixel: coordinates and color (rgba).

Function <i>stop</i> stops search. Useful if you found a pixel that was needed and want to stop search.

Function <i>skip</i> skips neighbors. When you call this function all neighbors of the visited pixel won't be added to the processing queue. Useful if you found boundary pixel of some shape, want to continue search within the shape and not to go beyond its boundaries.

## Example

We have an image with 3 markers:

<img src="https://raw.githubusercontent.com/AndreyGeonya/img-bfs/master/examples/markers/markers.jpg" width="440", height="280">

When user clicks to one of them we need to add mask on top of it:

<img src="https://raw.githubusercontent.com/AndreyGeonya/img-bfs/master/examples/markers/screenshot.png" width="440", height="280">

To implement this feature we need to get all pixels of the clicked marker and draw our mask in these pixel positions. Let's find pixels and draw mask on canvas overlay: 

    window.onload = function() {
        var img = document.getElementById('myImg'),
            canv = document.getElementById("canv"),
            ctx = canv.getContext("2d");
        
        ctx.fillStyle = "rgba(0, 0, 0, 0.3)";

        canv.onclick = function(e) {
            bfs(img, {x: e.offsetX, y: e.offsetY}, {
                onvisit: draw
            });
        }

        function draw(e) {
            ctx.beginPath();
            ctx.arc(e.pixel.coord.x, e.pixel.coord.y, 1, 0, 2 * Math.PI, true);
            ctx.fill();
            ctx.closePath();
            // skip neighbors of the white pixels,
            // white pixels are not belongs to the marker
            if (e.pixel.color[0] === 255 && 
                e.pixel.color[1] === 255 && 
                e.pixel.color[2] === 255) {
                e.skip();
            }
        }
    }

See full code of this example <a href="https://github.com/AndreyGeonya/img-bfs/blob/master/examples/markers/index.html">here</a>.

## TODO
* Add more examples

## Changelog

### 0.1.1 &mdash; 26.10.2014

* Blacklist added
* package.json added

### 0.1.0 &mdash; 15.08.2014

* First Img-bfs release