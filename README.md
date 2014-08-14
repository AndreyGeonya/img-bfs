Img-bfs is a JavaScript library for breadth-first traversal in image.

## API

### Main function

The main function of the library is 

<b>bfs</b>(&lt;HTMLImageElement | HTMLCanvasElement&gt; <i>img</i>, &lt;Coord | Array&gt; <i>startCoord</i>, &lt;Object&gt; <i>eventMap</i>);

It starts search in <i>img</i> image from <i>startCoord</i> coordinate(s).

### Events

You can subscribe to events via eventMap param. For example:

    bfs(img, {x: 10, y: 10}, {
        onvisit: function(e) {
            console.log(e);
        }
    });

Currently just <i>onvisit</i> event supported. This event rises when img-bfs visits each pixel. Event object structure:

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

Function <i>skip</i> skips neighbors. When you call this function all neighbors of the visited pixel won't be added to the processing queue. Useful if you found boundary pixel of some image and want to continue search within the image, but not to go beyond its boundaries.

## Changelog

### 1.0.0 &mdash; 15.08.2014

* First Img-bfs release