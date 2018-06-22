# SVG - What is it? Pros & Cons
![
](https://lh3.googleusercontent.com/-vg3_sNKTdDI8hmlU8d_OVT-Zo42IyDsE-aMX1Fjo7MlOA-4UeGylJ-3mWLYYQnujXjoysdedXBz=s800 "SVG")

I'm pretty sure that many of you are familiar with several kinds of raster graphic formats such as `jpeg`, `png`, `gif`, etc. and fond of using them as main assets for your websites.
Have you ever heard of SVG since the day it first appeared back in 1999? You might start to stick with it in your next project after reading this introduction.

SVGs are literally Scalable Vector Graphic. Unlike raster images which are predefined by a map of pixels (bitmap), SVGs instead describe what should be displayed and how to draw them out (specifications) using `xml` syntax. For example, you might say, draw me a circle of radius 20 pixels rather than providing a map of 20x20 pixels defining that circle. 

As a result, the dimensions of SVG graphics are much more flexible and easy to scale without blurring out image details. This characteristic really makes SVG stand out when dealing with a great variation in screen sizes and resolutions. SVGs help us to provide graphics that look good across all that kinds of devices.

## Pros
When comparing to raster twins, SVGs turn out to have a lot of advantages:

### Super lightweight
Raster images,, together with their bitmap, carry a lot of data along. In the figure below, believe it or not, the size of the image (a) and (b) are respectively `11KB` and `363 bytes (~ 0.35KB)`. 

![
](https://lh3.googleusercontent.com/gASuB4UEEag1Yz5Qh8Hq7lnmh0l9MAns9tl40asP83NTPC1M-phMYsNmtmApSXeIczCvmzS6lXU5=s900 "Super lightweight")

Take a closer look at the edges, image (b) even keeps more details than (a).

### Flexibility
We can interact with SVGs just like the way we do with other DOM elements using CSS and Javascript. Yep, you heard it right. It makes SVGs an ideal format for animations.

![enter image description here](https://lh3.googleusercontent.com/tfoSIgIozmEtB6lHQyYC-smJTjt3XXX6QqFwPxlMiH-SZSFi2NOQeHKblxqBAUC8MD5bQJ5sJyxF "SVG Animation")

### Readable content
It's possible to view SVGs content in any browser. You can even edit SVG component's specification right in a text editor (of course if you have enough SVG knowledge). For instance, the code of a simple circle looks like this:

<svg height="100" width="100">
  <circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" />
</svg>

```html
<svg height="100" width="100">
  <circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" />
</svg> 
```
### Coding knowledge is not a must-have
Popular graphics editing tools such as Photoshop, Illustrator, etc. can export images as SVGs at your ease.

## Cons
* The file size is growing really fast, especially when the image contains a huge number of components inside.
* SVGs just work with two-dimensional images only
* SVGs are best for logos and icons. If you're thinking of art photos which might have tons of tones of a single color, you should pick something else, such as JPEG.

## Conclusions
Its obvious that the pro far way out the cons. SVGs might be worthy of consideration when you start preparing graphic assets for your web projects.
