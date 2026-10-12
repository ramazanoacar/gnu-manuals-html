### 42.17 Images [¶](#Images-1)

To display an image in an Emacs buffer, you must first create an image
descriptor, then use it as a display specifier in the `display`
property of text that is displayed (see [The `display` Property](#Display-Property)).

Emacs is usually able to display images when it is run on a
graphical terminal. Images cannot be displayed in a text terminal, on
certain graphical terminals that lack the support for this, or if
Emacs is compiled without image support. You can use the function
`display-images-p` to determine if images can in principle be
displayed (see [Display Feature Testing](#Display-Feature-Testing)).

* [Image Formats](#Image-Formats)
* [Image Descriptors](#Image-Descriptors)
* [XBM Images](#XBM-Images)
* [XPM Images](#XPM-Images)
* [ImageMagick Images](#ImageMagick-Images)
* [SVG Images](#SVG-Images)
* [Other Image Types](#Other-Image-Types)
* [Defining Images](#Defining-Images)
* [Showing Images](#Showing-Images)
* [Multi-Frame Images](#Multi_002dFrame-Images)
* [Image Cache](#Image-Cache)

---

Next: [Image Descriptors](#Image-Descriptors), Up: [Images](#Images)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.17.1 Image Formats [¶](#Image-Formats-1)

Emacs can display a number of different image formats. Some of
these image formats are supported only if particular support libraries
are installed. On some platforms, Emacs can load support libraries on
demand; if so, the variable `dynamic-library-alist` can be used
to modify the set of known names for these dynamic libraries.
See [Dynamically Loaded Libraries](#Dynamic-Libraries).

Supported image formats (and the required support libraries) include
PBM and XBM (which do not depend on support libraries and are always
available), XPM (`libXpm`), GIF (`libgif` or
`libungif`), JPEG (`libjpeg`), TIFF (`libtiff`), PNG
(`libpng`), SVG (`librsvg`), and WebP (`libwebp`).

Each of these image formats is associated with an *image type
symbol*. The symbols for the above formats are, respectively,
`pbm`, `xbm`, `xpm`, `gif`, `jpeg`,
`tiff`, `png`, `svg`, and `webp`.

On some platforms, the built-in image support that doesn’t require
any optional libraries includes BMP images.[34](#FOOT34)

Furthermore, if you build Emacs with ImageMagick
(`libMagickWand`) support, Emacs can display any image format
that ImageMagick can. See [ImageMagick Images](#ImageMagick-Images). All images
displayed via ImageMagick have type symbol `imagemagick`.

Variable: **image-types** [¶](#index-image_002dtypes)
:   This variable contains a list of type symbols for image formats which
    are potentially supported in the current configuration.

    “Potentially” means that Emacs knows about the image types, not
    necessarily that they can be used (for example, they could depend on
    unavailable dynamic libraries). To know which image types are really
    available, use `image-type-available-p`.

Function: **image-type-available-p** type [¶](#index-image_002dtype_002davailable_002dp)
:   This function returns non-`nil` if images of type type can
    be loaded and displayed. type must be an image type symbol.

    For image types whose support libraries are statically linked, this
    function always returns `t`. For image types whose support
    libraries are dynamically loaded, it returns `t` if the library
    could be loaded and `nil` otherwise.

---

Next: [XBM Images](#XBM-Images), Previous: [Image Formats](#Image-Formats), Up: [Images](#Images)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.17.2 Image Descriptors [¶](#Image-Descriptors-1)

An *image descriptor* is a list which specifies the underlying
data for an image, and how to display it. It is typically used as the
value of a `display` overlay or text property (see [Other Display Specifications](#Other-Display-Specs)); but See [Showing Images](#Showing-Images), for convenient helper
functions to insert images into buffers.

Each image descriptor has the form `(image . props)`,
where props is a property list of alternating keyword symbols
and values, including at least the pair `:type type` that
specifies the image type.

Image descriptors which define image dimensions, `:width`,
`:height`, `:max-width` and `:max-height`, may take
either an integer, which represents the dimension in pixels, or a pair
`(value . em)`, where value is the dimension’s
length in *ems*[35](#FOOT35). One em is equivalent to the size of the font
and value may be an integer or a float. Also, dimension can be
specified in `(value . ch)` and `(value . cw)`
forms, where `ch` means height of the canonical character and
`cw` means width of the canonical character.

The following is a list of properties that are meaningful for all
image types (there are also properties which are meaningful only for
certain image types, as documented in the following subsections):

`:type type`
:   The image type.
    See [Image Formats](#Image-Formats).
    Every image descriptor must include this property.

`:file file`
:   This says to load the image from file file. If file is
    not an absolute file name, it is expanded relative to each of the
    directories mentioned by `image-load-path` (see [Defining Images](#Defining-Images)).

`:data data`
:   This specifies the raw image data. Each image descriptor must have
    either `:data` or `:file`, but not both.

    For most image types, the value of a `:data` property should be a
    string containing the image data. Some image types do not support
    `:data`; for some others, `:data` alone is not enough, so
    you need to use other image properties along with `:data`. See
    the following subsections for details.

`:margin margin`
:   This specifies how many pixels to add as an extra margin around the
    image. The value, margin, must be a non-negative number, or a
    pair `(x . y)` of such numbers. If it is a pair,
    x specifies how many pixels to add horizontally, and y
    specifies how many pixels to add vertically. If `:margin` is not
    specified, the default is zero.

`:ascent ascent`
:   This specifies the amount of the image’s height to use for its
    ascent—that is, the part above the baseline. The value,
    ascent, must be a number in the range 0 to 100, or the symbol
    `center`.

    If ascent is a number, that percentage of the image’s height is
    used for its ascent.

    If ascent is `center`, the image is vertically centered
    around a centerline which would be the vertical centerline of text drawn
    at the position of the image, in the manner specified by the text
    properties and overlays that apply to the image.

    If this property is omitted, it defaults to 50.

`:relief relief`
:   This adds a shadow rectangle around the image. The value,
    relief, specifies the width of the shadow lines, in pixels. If
    relief is negative, shadows are drawn so that the image appears
    as a pressed button; otherwise, it appears as an unpressed button.

`:width width, :height height`
:   The `:width` and `:height` keywords are used for scaling the
    image. If only one of them is specified, the other one will be
    calculated so as to preserve the aspect ratio. If both are specified,
    aspect ratio may not be preserved.

`:max-width max-width, :max-height max-height`
:   The `:max-width` and `:max-height` keywords are used for
    scaling if the size of the image exceeds these values. If
    `:width` is set, it will have precedence over `max-width`,
    and if `:height` is set, it will have precedence over
    `max-height`, but you can otherwise mix these keywords as you
    wish.

    If both `:max-width` and `:height` are specified, but
    `:width` is not, preserving the aspect ratio might require that
    width exceeds `:max-width`. If this happens, scaling will use a
    smaller value for the height so as to preserve the aspect ratio while
    not exceeding `:max-width`. Similarly when both
    `:max-height` and `:width` are specified, but `:height`
    is not. For example, if you have a 200x100 image and specify that
    `:width` should be 400 and `:max-height` should be 150,
    you’ll end up with an image that is 300x150: Preserving the aspect
    ratio and not exceeding the “max” setting. This combination of
    parameters is a useful way of saying “display this image as large as
    possible, but no larger than the available display area”.

`:scale scale`
:   This should be a scaling factor for the image, a number. Values higher
    than 1 mean to increase the image size, and lower values mean to
    decrease the size, by multiplying both the width and height of the image
    by the factor. For instance, a value of 0.25 will make the image a
    quarter size of what it originally was. If the scaling makes the image
    larger than specified by `:max-width` or `:max-height`, the
    resulting size will not exceed those two values. If both `:scale`
    and `:height`/`:width` are specified, the height/width will be
    adjusted by the specified scaling factor.

    The value of scale can also be the symbol `default`, which
    means to use the value of `image-scaling-factor`. If that value is
    a number, it is the scale factor to use; if it is `auto` (the
    default), it means to compute the scaling factor based on pixel size of
    the font used by the frame’s default face (see [Low-Level Font Representation](#Low_002dLevel-Font)).
    Specifically, if the pixel width of the default face’s font is greater
    than 10, the image is enlarged by the factor computed as the ratio of
    the font width to 10; if the font width is 10 pixels or less, the image
    is not scaled. For example, if the default font’s width is 15, the
    image will be scaled by the factor 1.5.

    If scale is not provided, `create-image` scales the image
    according to the value of `image-scaling-factor`.

`:rotation angle`
:   Specifies a rotation angle in degrees. Only multiples of 90 degrees
    are supported, unless the image type is `imagemagick`. Positive
    values rotate clockwise, negative values counter-clockwise. Rotation
    is performed after scaling and cropping.

`:flip flip`
:   If this is `t`, the image will be horizontally flipped.
    Currently it has no effect if the image type is `imagemagick`.
    Vertical flipping can be achieved by rotating the image 180 degrees
    and toggling this value.

`:transform-smoothing smooth`
:   If this is `t`, any image transform will have smoothing applied;
    if `nil`, no smoothing will be applied. The exact algorithm used
    is platform dependent, but should be equivalent to bilinear
    filtering. Disabling smoothing will use the nearest neighbor
    algorithm.

    If this property is not specified, `create-image` will use the
    `image-transform-smoothing` user option to say whether smoothing
    should be done or not. This option can be `nil` (no smoothing),
    `t` (use smoothing) or a predicate function that’s called with
    the image object as the only parameter, and should return either
    `nil` or `t`. The default is for down-scaling to apply
    smoothing, and for large up-scaling to not apply smoothing.

`:index frame`
:   See [Multi-Frame Images](#Multi_002dFrame-Images).

`:conversion algorithm`
:   This specifies a conversion algorithm that should be applied to the
    image before it is displayed; the value, algorithm, specifies
    which algorithm.

    `laplace`

    `emboss`
    :   Specifies the Laplace edge detection algorithm, which blurs out small
        differences in color while highlighting larger differences. People
        sometimes consider this useful for displaying the image for a
        disabled button.

    `(edge-detection :matrix matrix :color-adjust adjust)` [¶](#index-edge-detection_002c-images)
    :   Specifies a general edge-detection algorithm. matrix must be
        either a nine-element list or a nine-element vector of numbers. A pixel
        at position *x/y* in the transformed image is computed from
        original pixels around that position. matrix specifies, for each
        pixel in the neighborhood of *x/y*, a factor with which that pixel
        will influence the transformed pixel; element *0* specifies the
        factor for the pixel at *x-1/y-1*, element *1* the factor for
        the pixel at *x/y-1* etc., as shown below:

        ```
          (x-1/y-1  x/y-1  x+1/y-1
           x-1/y    x/y    x+1/y
           x-1/y+1  x/y+1  x+1/y+1)
        ```

        The resulting pixel is computed from the color intensity of the color
        resulting from summing up the RGB values of surrounding pixels,
        multiplied by the specified factors, and dividing that sum by the sum
        of the factors’ absolute values.

        Laplace edge-detection currently uses a matrix of

        ```
          (1  0  0
           0  0  0
           0  0 -1)
        ```

        Emboss edge-detection uses a matrix of

        ```
          ( 2 -1  0
           -1  0  1
            0  1 -2)
        ```

    `disabled`
    :   Specifies transforming the image so that it looks disabled.

`:mask mask`
:   If mask is `heuristic` or `(heuristic bg)`, build
    a clipping mask for the image, so that the background of a frame is
    visible behind the image. If bg is not specified, or if bg
    is `t`, determine the background color of the image by looking at
    the four corners of the image, assuming the most frequently occurring
    color from the corners is the background color of the image. Otherwise,
    bg must be a list `(red green blue)`
    specifying the color to assume for the background of the image.

    If mask is `nil`, remove a mask from the image, if it has
    one. Images in some formats include a mask which can be removed by
    specifying `:mask nil`.

`:pointer shape`
:   This specifies the pointer shape when the mouse pointer is over this
    image. See [Pointer Shape](#Pointer-Shape), for available pointer shapes.

`:map map` [¶](#index-image-maps)
:   This associates an image map of *hot spots* with this image.

    An image map is an alist where each element has the format
    `(area id plist)`. An area is specified
    as either a rectangle, a circle, or a polygon.

    A rectangle is a cons
    `(rect . ((x0 . y0) . (x1 . y1)))`
    which specifies the pixel coordinates of the upper left and bottom right
    corners of the rectangle area.

    A circle is a cons
    `(circle . ((x0 . y0) . r))`
    which specifies the center and the radius of the circle; r may
    be a float or integer.

    A polygon is a cons
    `(poly . [x0 y0 x1 y1 ...])`
    where each pair in the vector describes one corner in the polygon.

    When the mouse pointer lies on a hot-spot area of an image, the
    plist of that hot-spot is consulted; if it contains a `help-echo`
    property, that defines a tool-tip for the hot-spot, and if it contains
    a `pointer` property, that defines the shape of the mouse cursor when
    it is on the hot-spot.
    See [Pointer Shape](#Pointer-Shape), for available pointer shapes.

    When you click the mouse when the mouse pointer is over a hot-spot, an
    event is composed by combining the id of the hot-spot with the
    mouse event; for instance, `[area4 mouse-1]` if the hot-spot’s
    id is `area4`.

    Note that the map’s coordinates should reflect the displayed image
    after all transforms have been done (rotation, scaling and so on), and
    also note that Emacs (by default) performs auto-scaling of images, so
    to make things match up, you should either specify `:scale 1.0`
    when creating the image, or use the result of
    `image-compute-scaling-factor` to compute the elements of the
    map.

    When an image’s `:scale`, `:rotation`, or `:flip` is
    changed, `:map` will be recomputed based on the value of
    `:original-map` and the values of those transformation.

`:original-map original-map` [¶](#index-original-image-map)
:   This specifies the untransformed image map which will be used to
    recompute `:map` after the image’s `:scale`, `:rotation`,
    or `:flip` is changed.

    If `:original-map` is not specified when creating an image with
    `create-image`, it will be computed based on the supplied
    `:map`, as well as any of `:scale`, `:rotation`, or
    `:flip` which are non-`nil`.

    Conversely, if `:original-map` is specified but `:map` is not,
    `:map` will be computed based on `:original-map`,
    `:scale`, `:rotation`, and `:flip`.

    User Option: **image-recompute-map-p** [¶](#index-image_002drecompute_002dmap_002dp)
    :   Set this user option to nil to prevent Emacs from automatically
        recomputing an image `:map` based on its `:original-map`.

Function: **image-mask-p** spec &optional frame [¶](#index-image_002dmask_002dp)
:   This function returns `t` if image spec has a mask bitmap.
    frame is the frame on which the image will be displayed.
    frame `nil` or omitted means to use the selected frame
    (see [Input Focus](#Input-Focus)).

Function: **image-transforms-p** &optional frame [¶](#index-image_002dtransforms_002dp)
:   This function returns non-`nil` if frame supports image
    scaling and rotation. frame `nil` or omitted means to use
    the selected frame (see [Input Focus](#Input-Focus)). The returned list includes
    symbols that indicate which image transform operations are supported:

    `scale`
    :   Image scaling is supported by frame via the `:scale`,
        `:width`, `:height`, `:max-width`, and
        `:max-height` properties.

    `rotate90`
    :   Image rotation is supported by frame if the rotation angle is an
        integral multiple of 90 degrees.

    If image transforms are not supported, `:rotation`, `:crop`,
    `:width`, `:height`, `:scale`, `:max-width` and
    `:max-height` will only be usable through ImageMagick, if
    available (see [ImageMagick Images](#ImageMagick-Images)).

---

Next: [XPM Images](#XPM-Images), Previous: [Image Descriptors](#Image-Descriptors), Up: [Images](#Images)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.17.3 XBM Images [¶](#XBM-Images-1)

To use XBM format, specify `xbm` as the image type. This image
format doesn’t require an external library, so images of this type are
always supported.

Additional image properties supported for the `xbm` image type are:

`:foreground foreground`
:   The value, foreground, should be a string specifying the image
    foreground color, or `nil` for the default color. This color is
    used for each pixel in the XBM that is 1. The default is the frame’s
    foreground color.

`:background background`
:   The value, background, should be a string specifying the image
    background color, or `nil` for the default color. This color is
    used for each pixel in the XBM that is 0. The default is the frame’s
    background color.

To specify an XBM image using data within Emacs instead of an
external file, use the following properties:

`:data data`
:   The value, data, specifies the contents of the image.
    There are three formats you can use for data:

    * A vector of strings or bool-vectors, each specifying one line of the
      image. Do specify `:data-height` and `:data-width`.
    * A string containing the same byte sequence as an XBM file would contain.
    * A string or a bool-vector containing the bits of the image (plus
      perhaps some extra bits at the end that will not be used). It should
      contain at least `stride * height` bits, where
      stride is the smallest multiple of 8 greater than or equal to
      the width of the image. In this case, you should specify
      `:data-height`, `:data-width` and `:stride`, both to
      indicate that the string contains just the bits rather than a whole
      XBM file, and to specify the size of the image.

`:stride stride`
:   The number of bool vector entries stored for each row; the smallest
    multiple of 8 greater than or equal to width.

---

Next: [ImageMagick Images](#ImageMagick-Images), Previous: [XBM Images](#XBM-Images), Up: [Images](#Images)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.17.4 XPM Images [¶](#XPM-Images-1)

To use XPM format, specify `xpm` as the image type. The
additional image property `:color-symbols` is also meaningful with
the `xpm` image type:

`:color-symbols symbols`
:   The value, symbols, should be an alist whose elements have the
    form `(name . color)`. In each element, name is
    the name of a color as it appears in the image file, and color
    specifies the actual color to use for displaying that name.

---

Next: [SVG Images](#SVG-Images), Previous: [XPM Images](#XPM-Images), Up: [Images](#Images)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.17.5 ImageMagick Images [¶](#ImageMagick-Images-1)

If your Emacs build has ImageMagick support, you can use the
ImageMagick library to load many image formats (see [File
Conveniences](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#File-Conveniences) in The GNU Emacs Manual). The image type symbol
for images loaded via ImageMagick is `imagemagick`, regardless of
the actual underlying image format.

To check for ImageMagick support, use the following:

```
(image-type-available-p 'imagemagick)
```

Function: **imagemagick-types** [¶](#index-imagemagick_002dtypes)
:   This function returns a list of image file extensions supported by the
    current ImageMagick installation. Each list element is a symbol
    representing an internal ImageMagick name for an image type, such as
    `BMP` for `.bmp` images.

User Option: **imagemagick-enabled-types** [¶](#index-imagemagick_002denabled_002dtypes)
:   The value of this variable is a list of ImageMagick image types which
    Emacs may attempt to render using ImageMagick. Each list element
    should be one of the symbols in the list returned by
    `imagemagick-types`, or an equivalent string. Alternatively, a
    value of `t` enables ImageMagick for all possible image types.
    Regardless of the value of this variable,
    `imagemagick-types-inhibit` (see below) takes precedence.

User Option: **imagemagick-types-inhibit** [¶](#index-imagemagick_002dtypes_002dinhibit)
:   The value of this variable lists the ImageMagick image types which
    should never be rendered using ImageMagick, regardless of the value of
    `imagemagick-enabled-types`. A value of `t` disables
    ImageMagick entirely.

Variable: **image-format-suffixes** [¶](#index-image_002dformat_002dsuffixes)
:   This variable is an alist mapping image types to file name extensions.
    Emacs uses this in conjunction with the `:format` image property
    (see below) to give a hint to the ImageMagick library as to the type
    of an image. Each element has the form `(type
    extension)`, where type is a symbol specifying an image
    content-type, and extension is a string that specifies the
    associated file name extension.

Images loaded with ImageMagick support the following additional
image descriptor properties:

`:background background`
:   background, if non-`nil`, should be a string specifying a
    color, which is used as the image’s background color if the image
    supports transparency. If the value is `nil`, it defaults to the
    frame’s background color.

`:format type`
:   The value, type, should be a symbol specifying the type of the
    image data, as found in `image-format-suffixes`. This is used
    when the image does not have an associated file name, to provide a
    hint to ImageMagick to help it detect the image type.

`:crop geometry`
:   The value of geometry should be a list of the form
    `(width height x y)`. width and
    height specify the width and height of the cropped image. If
    x is a positive number it specifies the offset of the cropped
    area from the left of the original image, and if negative the offset
    from the right. If y is a positive number it specifies the
    offset from the top of the original image, and if negative from the
    bottom. If x or y are `nil` or unspecified the crop
    area will be centered on the original image.

    If the crop area is outside or overlaps the edge of the image it will
    be reduced to exclude any areas outside of the image. This means it
    is not possible to use `:crop` to increase the size of the image
    by entering large width or height values.

    Cropping is performed after scaling but before rotation.

---

Next: [Other Image Types](#Other-Image-Types), Previous: [ImageMagick Images](#ImageMagick-Images), Up: [Images](#Images)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.17.6 SVG Images [¶](#SVG-Images-1)

SVG (Scalable Vector Graphics) is an XML format for specifying images.
SVG images support the following additional image descriptor
properties:

`:foreground foreground`
:   foreground, if non-`nil`, should be a string specifying a
    color, which is used as the CSS ‘currentcolor’ value. If the value is
    `nil`, it defaults to the current face’s foreground color.

`:background background`
:   background, if non-`nil`, should be a string specifying a
    color, which is used as the image’s background color if the image
    supports transparency. If the value is `nil`, it defaults to the
    current face’s background color.

`:css css`
:   css, if non-`nil`, should be a string specifying the CSS to
    override the default CSS used when generating the image.

#### SVG library [¶](#SVG-library)

If your Emacs build has SVG support, you can create and manipulate
these images with the following functions from the `svg.el`
library.

Function: **svg-create** width height &rest args [¶](#index-svg_002dcreate)
:   Create a new, empty SVG image with the specified dimensions.
    args is an argument plist with you can specify following:

    `:stroke-width`
    :   The default width (in pixels) of any lines created.

    `:stroke`
    :   The default stroke color on any lines created.

    This function returns an *SVG object*, a Lisp data structure that
    specifies an SVG image, and all the following functions work on that
    structure. The argument svg in the following functions
    specifies such an SVG object.

Function: **svg-gradient** svg id type stops [¶](#index-svg_002dgradient)
:   Create a gradient in svg with identifier id. type
    specifies the gradient type, and can be either `linear` or
    `radial`. stops is a list of percentage/color pairs.

    The following will create a linear gradient that goes from red at the
    start, to green 25% of the way, to blue at the end:

    ```
    (svg-gradient svg "gradient1" 'linear
                  '((0 . "red") (25 . "green") (100 . "blue")))
    ```

    The gradient created (and inserted into the SVG object) can later be
    used by all functions that create shapes.

All the following functions take an optional list of keyword
parameters that alter the various attributes from their default
values. Valid attributes include:

`:stroke-width`
:   The width (in pixels) of lines drawn, and outlines around solid
    shapes.

`:stroke-color`
:   The color of lines drawn, and outlines around solid shapes.

`:fill-color`
:   The color used for solid shapes.

`:id`
:   The identified of the shape.

`:gradient`
:   If given, this should be the identifier of a previously defined
    gradient object.

`:clip-path`
:   Identifier of a clip path.

Function: **svg-rectangle** svg x y width height &rest args [¶](#index-svg_002drectangle)
:   Add to svg a rectangle whose upper left corner is at
    position x/y and whose size is width/height.

    ```
    (svg-rectangle svg 100 100 500 500 :gradient "gradient1")
    ```

Function: **svg-circle** svg x y radius &rest args [¶](#index-svg_002dcircle)
:   Add to svg a circle whose center is at x/y and whose
    radius is radius.

Function: **svg-ellipse** svg x y x-radius y-radius &rest args [¶](#index-svg_002dellipse)
:   Add to svg an ellipse whose center is at x/y, and
    whose horizontal radius is x-radius and the vertical radius is
    y-radius.

Function: **svg-line** svg x1 y1 x2 y2 &rest args [¶](#index-svg_002dline)
:   Add to svg a line that starts at x1/y1 and extends
    to x2/y2.

Function: **svg-polyline** svg points &rest args [¶](#index-svg_002dpolyline)
:   Add to svg a multiple-segment line (a.k.a. “polyline”) that
    goes through points, which is a list of X/Y position pairs.

    ```
    (svg-polyline svg '((200 . 100) (500 . 450) (80 . 100))
                  :stroke-color "green")
    ```

Function: **svg-polygon** svg points &rest args [¶](#index-svg_002dpolygon)
:   Add a polygon to svg where points is a list of X/Y pairs
    that describe the outer circumference of the polygon.

    ```
    (svg-polygon svg '((100 . 100) (200 . 150) (150 . 90))
                 :stroke-color "blue" :fill-color "red")
    ```

Function: **svg-path** svg commands &rest args [¶](#index-svg_002dpath)
:   Add the outline of a shape to svg according to commands,
    see [SVG Path Commands](#SVG-Path-Commands).

    Coordinates by default are absolute. To use coordinates relative to
    the last position, or – initially – to the origin, set the attribute
    :relative to `t`. This attribute can be specified for the
    function or for individual commands. If specified for the function,
    then all commands use relative coordinates by default. To make an
    individual command use absolute coordinates, set :relative to
    `nil`.

    ```
    (svg-path svg
    	  '((moveto ((100 . 100)))
    	    (lineto ((200 . 0) (0 . 200) (-200 . 0)))
    	    (lineto ((100 . 100)) :relative nil))
    	  :stroke-color "blue"
    	  :fill-color "lightblue"
    	  :relative t)
    ```

Function: **svg-text** svg text &rest args [¶](#index-svg_002dtext)
:   Add the specified text to svg.

    ```
    (svg-text
     svg "This is a text"
     :font-size "40"
     :font-weight "bold"
     :stroke "black"
     :fill "white"
     :font-family "impact"
     :letter-spacing "4pt"
     :x 300
     :y 400
     :stroke-width 1)
    ```

Function: **svg-embed** svg image image-type datap &rest args [¶](#index-svg_002dembed)
:   Add an embedded (raster) image to svg. If datap is
    `nil`, image should be a file name; otherwise it should be a
    string containing the image data as raw bytes. image-type should be a
    MIME image type, for instance `"image/jpeg"`.

    ```
    (svg-embed svg "~/rms.jpg" "image/jpeg" nil
               :width "100px" :height "100px"
               :x "50px" :y "75px")
    ```

Function: **svg-embed-base-uri-image** svg relative-filename &rest args [¶](#index-svg_002dembed_002dbase_002duri_002dimage)
:   To svg add an embedded (raster) image placed at
    relative-filename. relative-filename is searched inside
    `file-name-directory` of the `:base-uri` svg image property.
    `:base-uri` specifies a (possibly non-existing) file name of the
    svg image to be created, thus all the embedded files are searched
    relatively to the `:base-uri` filename’s directory. If
    `:base-uri` is omitted, then filename from where svg image is
    loaded is used. Using `:base-uri` improves the performance of
    embedding large images, comparing to `svg-embed`, because all the
    work is done directly by librsvg.

    ```
    ;; Embedding /tmp/subdir/rms.jpg and /tmp/another/rms.jpg
    (svg-embed-base-uri-image svg "subdir/rms.jpg"
               :width "100px" :height "100px"
               :x "50px" :y "75px")
    (svg-embed-base-uri-image svg "another/rms.jpg"
               :width "100px" :height "100px"
               :x "75px" :y "50px")
    (svg-image svg :scale 1.0
               :base-uri "/tmp/dummy"
               :width 175 :height 175)
    ```

Function: **svg-clip-path** svg &rest args [¶](#index-svg_002dclip_002dpath)
:   Add a clipping path to svg. If applied to a shape via the
    :clip-path property, parts of that shape which lie outside of
    the clipping path are not drawn.

    ```
    (let ((clip-path (svg-clip-path svg :id "foo")))
      (svg-circle clip-path 200 200 175))
    (svg-rectangle svg 50 50 300 300
                   :fill-color "red"
                   :clip-path "url(#foo)")
    ```

Function: **svg-node** svg tag &rest args [¶](#index-svg_002dnode)
:   Add the custom node tag to svg.

    ```
    (svg-node svg
              'rect
              :width 300 :height 200 :x 50 :y 100 :fill-color "green")
    ```

Function: **svg-remove** svg id [¶](#index-svg_002dremove)
:   Remove the element with identifier `id` from the `svg`.

Function: **svg-image** svg [¶](#index-svg_002dimage)
:   Finally, the `svg-image` takes an SVG object as its argument and
    returns an image object suitable for use in functions like
    `insert-image`.

Here’s a complete example that creates and inserts an image with a
circle:

```
(let ((svg (svg-create 400 400 :stroke-width 10)))
  (svg-gradient svg "gradient1" 'linear '((0 . "red") (100 . "blue")))
  (svg-circle svg 200 200 100 :gradient "gradient1"
                  :stroke-color "green")
  (insert-image (svg-image svg)))
```

#### SVG Path Commands [¶](#SVG-Path-Commands-1)

*SVG paths* allow creation of complex images by combining lines,
curves, arcs, and other basic shapes. The functions described below
allow invoking SVG path commands from a Lisp program.

Command: **moveto** points [¶](#index-moveto)
:   Move the pen to the first point in points. Additional points
    are connected with lines. points is a list of X/Y coordinate
    pairs. Subsequent `moveto` commands represent the start of a
    new *subpath*.

    ```
    (svg-path svg '((moveto ((200 . 100) (100 . 200) (0 . 100))))
              :fill "white" :stroke "black")
    ```

Command: **closepath** [¶](#index-closepath)
:   End the current subpath by connecting it back to its initial point. A
    line is drawn along the connection.

    ```
    (svg-path svg '((moveto ((200 . 100) (100 . 200) (0 . 100)))
                    (closepath)
                    (moveto ((75 . 125) (100 . 150) (125 . 125)))
                    (closepath))
              :fill "red" :stroke "black")
    ```

Command: **lineto** points [¶](#index-lineto)
:   Draw a line from the current point to the first element in
    points, a list of X/Y position pairs. If more than one point is
    specified, draw a polyline.

    ```
    (svg-path svg '((moveto ((200 . 100)))
                    (lineto ((100 . 200) (0 . 100))))
              :fill "yellow" :stroke "red")
    ```

Command: **horizontal-lineto** x-coordinates [¶](#index-horizontal_002dlineto)
:   Draw a horizontal line from the current point to the first element in
    x-coordinates. Specifying multiple coordinates is possible,
    although this usually doesn’t make sense.

    ```
    (svg-path svg '((moveto ((100 . 200)))
                    (horizontal-lineto (300)))
              :stroke "green")
    ```

Command: **vertical-lineto** y-coordinates [¶](#index-vertical_002dlineto)
:   Draw vertical lines.

    ```
    (svg-path svg '((moveto ((200 . 100)))
                    (vertical-lineto (300)))
              :stroke "green")
    ```

Command: **curveto** coordinate-sets [¶](#index-curveto)
:   Using the first element in coordinate-sets, draw a cubic Bézier
    curve from the current point. If there are multiple coordinate sets,
    draw a polybezier. Each coordinate set is a list of the form
    `(x1 y1 x2 y2 x y)`, where
    (x, y) is the curve’s end point. (x1, y1) and (x2, y2) are control points at the
    beginning and at the end, respectively.

    ```
    (svg-path svg '((moveto ((100 . 100)))
                    (curveto ((200 100 100 200 200 200)
                              (300 200 0 100 100 100))))
              :fill "transparent" :stroke "red")
    ```

Command: **smooth-curveto** coordinate-sets [¶](#index-smooth_002dcurveto)
:   Using the first element in coordinate-sets, draw a cubic Bézier
    curve from the current point. If there are multiple coordinate sets,
    draw a polybezier. Each coordinate set is a list of the form
    `(x2 y2 x y)`, where (x, y) is the curve’s end point and (x2, y2) is the
    corresponding control point. The first control point is the
    reflection of the second control point of the previous command
    relative to the current point, if that command was `curveto`
    or `smooth-curveto`. Otherwise the first control point
    coincides with the current point.

    ```
    (svg-path svg '((moveto ((100 . 100)))
                    (curveto ((200 100 100 200 200 200)))
                    (smooth-curveto ((0 100 100 100))))
              :fill "transparent" :stroke "blue")
    ```

Command: **quadratic-bezier-curveto** coordinate-sets [¶](#index-quadratic_002dbezier_002dcurveto)
:   Using the first element in coordinate-sets, draw a quadratic
    Bézier curve from the current point. If there are multiple coordinate
    sets, draw a polybezier. Each coordinate set is a list of the form
    `(x1 y1 x y)`, where (x, y) is the curve’s end point and (x1, y1) is the
    control point.

    ```
    (svg-path svg '((moveto ((200 . 100)))
                    (quadratic-bezier-curveto ((300 100 300 200)))
                    (quadratic-bezier-curveto ((300 300 200 300)))
                    (quadratic-bezier-curveto ((100 300 100 200)))
                    (quadratic-bezier-curveto ((100 100 200 100))))
              :fill "transparent" :stroke "pink")
    ```

Command: **smooth-quadratic-bezier-curveto** coordinate-sets [¶](#index-smooth_002dquadratic_002dbezier_002dcurveto)
:   Using the first element in coordinate-sets, draw a quadratic
    Bézier curve from the current point. If there are multiple coordinate
    sets, draw a polybezier. Each coordinate set is a list of the form
    `(x y)`, where (x, y) is the curve’s
    end point. The control point is the reflection of the control point
    of the previous command relative to the current point, if that command
    was `quadratic-bezier-curveto` or
    `smooth-quadratic-bezier-curveto`. Otherwise the control
    point coincides with the current point.

    ```
    (svg-path svg '((moveto ((200 . 100)))
                    (quadratic-bezier-curveto ((300 100 300 200)))
                    (smooth-quadratic-bezier-curveto ((200 300)))
                    (smooth-quadratic-bezier-curveto ((100 200)))
                    (smooth-quadratic-bezier-curveto ((200 100))))
              :fill "transparent" :stroke "lightblue")
    ```

Command: **elliptical-arc** coordinate-sets [¶](#index-elliptical_002darc)
:   Using the first element in coordinate-sets, draw an elliptical
    arc from the current point. If there are multiple coordinate sets,
    draw a sequence of elliptical arcs. Each coordinate set is a list of
    the form `(rx ry x y)`, where
    (x, y) is the end point of the ellipse, and
    (rx, ry) are its radii. Attributes may be appended to
    the list:

    `:x-axis-rotation`
    :   The angle in degrees by which the x-axis of the ellipse is rotated
        relative to the x-axis of the current coordinate system.

    `:large-arc`
    :   If set to `t`, draw an arc sweep greater than or equal to 180
        degrees. Otherwise, draw an arc sweep smaller than or equal to 180
        degrees.

    `:sweep`
    :   If set to `t`, draw an arc in *positive angle direction*.
        Otherwise, draw it in *negative angle direction*.

    ```
    (svg-path svg '((moveto ((200 . 250)))
                    (elliptical-arc ((75 75 200 350))))
              :fill "transparent" :stroke "red")
    (svg-path svg '((moveto ((200 . 250)))
                    (elliptical-arc ((75 75 200 350 :large-arc t))))
              :fill "transparent" :stroke "green")
    (svg-path svg '((moveto ((200 . 250)))
                    (elliptical-arc ((75 75 200 350 :sweep t))))
              :fill "transparent" :stroke "blue")
    (svg-path svg '((moveto ((200 . 250)))
                    (elliptical-arc ((75 75 200 350 :large-arc t
                                         :sweep t))))
              :fill "transparent" :stroke "gray")
    (svg-path svg '((moveto ((160 . 100)))
                    (elliptical-arc ((40 100 80 0)))
                    (elliptical-arc ((40 100 -40 -70
                                         :x-axis-rotation -120)))
                    (elliptical-arc ((40 100 -40 70
                                         :x-axis-rotation -240))))
              :stroke "pink" :fill "lightblue"
              :relative t)
    ```

---

Next: [Defining Images](#Defining-Images), Previous: [SVG Images](#SVG-Images), Up: [Images](#Images)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.17.7 Other Image Types [¶](#Other-Image-Types-1)

For PBM images, specify image type `pbm`. Color, gray-scale and
monochromatic images are supported. For mono PBM images, two additional
image properties are supported.

`:foreground foreground`
:   The value, foreground, should be a string specifying the image
    foreground color, or `nil` for the default color. This color is
    used for each pixel in the PBM that is 1. The default is the frame’s
    foreground color.

`:background background`
:   The value, background, should be a string specifying the image
    background color, or `nil` for the default color. This color is
    used for each pixel in the PBM that is 0. The default is the frame’s
    background color.

The remaining image types that Emacs can support are:

GIF
:   Image type `gif`.
    Supports the `:index` property. See [Multi-Frame Images](#Multi_002dFrame-Images).

JPEG
:   Image type `jpeg`.

PNG
:   Image type `png`.

TIFF
:   Image type `tiff`.
    Supports the `:index` property. See [Multi-Frame Images](#Multi_002dFrame-Images).

WebP
:   Image type `webp`.
    Supports the `:index` property. See [Multi-Frame Images](#Multi_002dFrame-Images).

---

Next: [Showing Images](#Showing-Images), Previous: [Other Image Types](#Other-Image-Types), Up: [Images](#Images)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.17.8 Defining Images [¶](#Defining-Images-1)

The functions `create-image`, `defimage` and
`find-image` provide convenient ways to create image descriptors.

Function: **create-image** file-or-data &optional type data-p &rest props [¶](#index-create_002dimage)
:   This function creates and returns an image descriptor which uses the
    data in file-or-data. file-or-data can be a file name or
    a string containing the image data; data-p should be `nil`
    for the former case, non-`nil` for the latter case. If
    file-or-data is a relative file name, the function will search
    for it in directories mentioned in `image-load-path`.

    The optional argument type is a symbol specifying the image type.
    If type is omitted or `nil`, `create-image` tries to
    determine the image type from the file’s first few bytes, or else
    from the file’s name.

    The remaining arguments, props, specify additional image
    properties—for example,

    ```
    (create-image "foo.xpm" 'xpm nil :mask 'heuristic)
    ```

    See [Image Descriptors](#Image-Descriptors), for the list of supported properties. Some
    properties are specific to certain image types, and are described in
    subsections specific to those types.

    The function returns `nil` if images of this type are not
    supported. Otherwise it returns an image descriptor.

Macro: **defimage** symbol specs &optional doc [¶](#index-defimage)
:   This macro defines symbol as an image name. The arguments
    specs is a list which specifies how to display the image.
    The third argument, doc, is an optional documentation string.

    Each argument in specs has the form of a property list, and each
    one should specify at least the `:type` property and either the
    `:file` or the `:data` property. The value of `:type`
    should be a symbol specifying the image type, the value of
    `:file` is the file to load the image from, and the value of
    `:data` is a string containing the actual image data. Here is an
    example:

    ```
    (defimage test-image
      ((:type xpm :file "~/test1.xpm")
       (:type xbm :file "~/test1.xbm")))
    ```

    `defimage` tests each argument, one by one, to see if it is
    usable—that is, if the type is supported and the file exists. The
    first usable argument is used to make an image descriptor which is
    stored in symbol.

    If none of the alternatives will work, then symbol is defined
    as `nil`.

Function: **image-property** image property [¶](#index-image_002dproperty)
:   Return the value of property in image. Properties can be
    set by using `setf`. Setting a property to `nil` will
    remove the property from the image.

Function: **find-image** specs [¶](#index-find_002dimage)
:   This function provides a convenient way to find an image satisfying one
    of a list of image specifications specs.

    Each specification in specs is a property list with contents
    depending on image type. All specifications must at least contain the
    properties `:type type` and either `:file file`
    or `:data data`, where type is a symbol specifying
    the image type, e.g., `xbm`, file is the file to load the
    image from, and data is a string containing the actual image data.
    The first specification in the list whose type is supported, and
    file exists, is used to construct the image specification to be
    returned. If no specification is satisfied, `nil` is returned.

    The image is looked for in `image-load-path`.

User Option: **image-load-path** [¶](#index-image_002dload_002dpath)
:   This variable’s value is a list of locations in which to search for
    image files. If an element is a string or a variable symbol whose
    value is a string, the string is taken to be the name of a directory
    to search. If an element is a variable symbol whose value is a list,
    that is taken to be a list of directories to search.

    The default is to search in the `images` subdirectory of the
    directory specified by `data-directory`, then the directory
    specified by `data-directory`, and finally in the directories in
    `load-path`. Subdirectories are not automatically included in
    the search, so if you put an image file in a subdirectory, you have to
    supply the subdirectory explicitly. For example, to find the
    image `images/foo/bar.xpm` within `data-directory`, you
    should specify the image as follows:

    ```
    (defimage foo-image '((:type xpm :file "foo/bar.xpm")))
    ```

Function: **image-load-path-for-library** library image &optional path no-error [¶](#index-image_002dload_002dpath_002dfor_002dlibrary)
:   This function returns a suitable search path for images used by the
    Lisp package library.

    The function searches for image first using `image-load-path`,
    excluding `data-directory/images`, and then in
    `load-path`, followed by a path suitable for library, which
    includes `../../etc/images` and `../etc/images` relative to
    the library file itself, and finally in
    `data-directory/images`.

    Then this function returns a list of directories which contains first
    the directory in which image was found, followed by the value of
    `load-path`. If path is given, it is used instead of
    `load-path`.

    If no-error is non-`nil` and a suitable path can’t be
    found, don’t signal an error. Instead, return a list of directories as
    before, except that `nil` appears in place of the image directory.

    Here is an example of using `image-load-path-for-library`:

    ```
    (defvar image-load-path) ; shush compiler
    (let* ((load-path (image-load-path-for-library
                        "mh-e" "mh-logo.xpm"))
           (image-load-path (cons (car load-path)
                                  image-load-path)))
      (mh-tool-bar-folder-buttons-init))
    ```

Images are automatically scaled when created based on the
`image-scaling-factor` variable. The value is either a floating
point number (where numbers higher than 1 means to increase the size
and lower means to shrink the size), or the symbol `auto`, which
will compute a scaling factor based on the font pixel size. See [Image Descriptors](#Image-Descriptors).

---

Next: [Multi-Frame Images](#Multi_002dFrame-Images), Previous: [Defining Images](#Defining-Images), Up: [Images](#Images)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.17.9 Showing Images [¶](#Showing-Images-1)

You can use an image descriptor by setting up the `display`
property yourself, but it is easier to use the functions in this
section.

Function: **insert-image** image &optional string area slice inhibit-isearch [¶](#index-insert_002dimage)
:   This function inserts image in the current buffer at point. The
    value image should be an image descriptor; it could be a value
    returned by `create-image`, or the value of a symbol defined with
    `defimage`. The argument string specifies the text to put
    in the buffer to hold the image. If it is omitted or `nil`,
    `insert-image` uses `" "` by default.

    The argument area specifies whether to put the image in a margin.
    If it is `left-margin`, the image appears in the left margin;
    `right-margin` specifies the right margin. If area is
    `nil` or omitted, the image is displayed at point within the
    buffer’s text.

    The argument slice specifies a slice of the image to insert. If
    slice is `nil` or omitted the whole image is inserted.
    (However, note that images are chopped on display at the window’s
    right edge, because wrapping images is not supported.) Otherwise,
    slice is a list `(x y width
    height)` which specifies the x and y positions and
    width and height of the image area to insert. Integer
    values are in units of pixels. A floating-point number in the range
    0.0–1.0 stands for that fraction of the width or height of the entire
    image.

    Internally, this function inserts string in the buffer, and gives
    it a `display` property which specifies image. See [The `display` Property](#Display-Property). By default, doing interactive searches in the buffer will
    consider string when searching. If inhibit-isearch is
    non-`nil`, this is inhibited.

Function: **insert-sliced-image** image &optional string area rows cols [¶](#index-insert_002dsliced_002dimage)
:   This function inserts image in the current buffer at point, like
    `insert-image`, but splits the image into rowsxcols
    equally sized slices.

    Emacs displays each slice as a
    separate image, and allows more intuitive scrolling up/down, instead of
    jumping up/down the entire image when paging through a buffer that
    displays (large) images.

Function: **put-image** image pos &optional string area [¶](#index-put_002dimage)
:   This function puts image image in front of pos in the
    current buffer. The argument pos should be an integer or a
    marker. It specifies the buffer position where the image should appear.
    The argument string specifies the text that should hold the image
    as an alternative to the default ‘`x`’.

    The argument image must be an image descriptor, perhaps returned
    by `create-image` or stored by `defimage`.

    The argument area specifies whether to put the image in a margin.
    If it is `left-margin`, the image appears in the left margin;
    `right-margin` specifies the right margin. If area is
    `nil` or omitted, the image is displayed at point within the
    buffer’s text.

    Internally, this function creates an overlay, and gives it a
    `before-string` property containing text that has a `display`
    property whose value is the image. (Whew! that was a mouthful…).
    It returns the created overlay upon success, and also sets its
    `put-image` property to `t`.

Function: **remove-images** start end &optional buffer [¶](#index-remove_002dimages)
:   This function removes images in buffer between positions
    start and end. If buffer is omitted or `nil`,
    images are removed from the current buffer.

    This removes only images that were put into buffer the way
    `put-image` does it, not images that were inserted with
    `insert-image` or in other ways.

Function: **image-size** spec &optional pixels frame [¶](#index-image_002dsize)
:   This function returns the size of an image as a pair
    `(width . height)`. spec is an image
    specification. pixels non-`nil` means return sizes measured
    in pixels, otherwise return sizes measured in the default character size
    of frame (see [Frame Font](#Frame-Font)). frame is the frame on which
    the image will be displayed. frame `nil` or omitted means
    use the selected frame (see [Input Focus](#Input-Focus)).

Variable: **max-image-size** [¶](#index-max_002dimage_002dsize)
:   This variable is used to define the maximum size of image that Emacs
    will load. Emacs will refuse to load (and display) any image that is
    larger than this limit.

    If the value is an integer, it directly specifies the maximum
    image height and width, measured in pixels. If it is floating
    point, it specifies the maximum image height and width
    as a ratio to the frame height and width. If the value is
    non-numeric, there is no explicit limit on the size of images.

    The purpose of this variable is to prevent unreasonably large images
    from accidentally being loaded into Emacs. It only takes effect the
    first time an image is loaded. Once an image is placed in the image
    cache, it can always be displayed, even if the value of
    `max-image-size` is subsequently changed (see [Image Cache](#Image-Cache)).

Function: **image-at-point-p** [¶](#index-image_002dat_002dpoint_002dp)
:   This function returns `t` if point is on an image, and `nil`
    otherwise.

Images inserted with the insertion functions above also get a local
keymap installed in the text properties (or overlays) that span the
displayed image. This keymap defines the following commands:

`i +` [¶](#index-image_002dincrease_002dsize)
:   Increase the image size (`image-increase-size`)

`i -` [¶](#index-image_002ddecrease_002dsize)
:   Decrease the image size (`image-decrease-size`).

`i r` [¶](#index-image_002drotate)
:   Rotate the image (`image-rotate`).

`i h` [¶](#index-image_002dflip_002dhorizontally)
:   Flip the image horizontally (`image-flip-horizontally`).

`i v` [¶](#index-image_002dflip_002dvertically)
:   Flip the image vertically (`image-flip-vertically`).

`i o` [¶](#index-image_002dsave)
:   Save the image to a file (`image-save`).

`i c` [¶](#index-image_002dcrop)
:   Interactively crop the image (`image-crop`).

`i x` [¶](#index-image_002dcut)
:   Interactively cut a rectangle from the image (`image-cut`).

See [Image Mode](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Image-Mode) in The GNU Emacs Manual, for more details
about these image-specific key bindings.

---

Next: [Image Cache](#Image-Cache), Previous: [Showing Images](#Showing-Images), Up: [Images](#Images)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.17.10 Multi-Frame Images [¶](#Multi_002dFrame-Images-1)

Some image files can contain more than one image. We say that there
are multiple “frames” in the image. At present, Emacs supports
multiple frames for GIF, TIFF, WebP, and certain ImageMagick formats
such as DJVM.

The frames can be used either to represent multiple pages (this is
usually the case with multi-frame TIFF files, for example), or to
create animation (usually the case with multi-frame GIF files).

A multi-frame image has a property `:index`, whose value is an
integer (counting from 0) that specifies which frame is being displayed.

Function: **image-multi-frame-p** image [¶](#index-image_002dmulti_002dframe_002dp)
:   This function returns non-`nil` if image contains more than
    one frame. The actual return value is a cons `(nimages
    . delay)`, where nimages is the number of frames and
    delay is the delay in seconds between them, or `nil`
    if the image does not specify a delay. Images that are intended to be
    animated usually specify a frame delay, whereas ones that are intended
    to be treated as multiple pages do not.

Function: **image-current-frame** image [¶](#index-image_002dcurrent_002dframe)
:   This function returns the index of the current frame number for
    image, counting from 0.

Function: **image-show-frame** image n &optional nocheck [¶](#index-image_002dshow_002dframe)
:   This function switches image to frame number n. It
    replaces a frame number outside the valid range with that of the end
    of the range, unless nocheck is non-`nil`. If image
    does not contain a frame with the specified number, the image displays
    as a hollow box.

Function: **image-animate** image &optional index limit [¶](#index-image_002danimate)
:   This function animates image. The optional integer index
    specifies the frame from which to start (default 0). The optional
    argument limit controls the length of the animation. If omitted
    or `nil`, the image animates once only; if `t` it loops
    forever; if a number animation stops after that many seconds.

Animation operates by means of a timer. Note that Emacs imposes a
minimum frame delay of 0.01 (`image-minimum-frame-delay`) seconds.
If the image itself does not specify a delay, Emacs uses
`image-default-frame-delay`.

Function: **image-animate-timer** image [¶](#index-image_002danimate_002dtimer)
:   This function returns the timer responsible for animating image,
    if there is one.

---

Previous: [Multi-Frame Images](#Multi_002dFrame-Images), Up: [Images](#Images)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.17.11 Image Cache [¶](#Image-Cache-1)

Emacs caches images so that it can display them again more
efficiently. When Emacs displays an image, it searches the image
cache for an existing image specification `equal` to the desired
specification. If a match is found, the image is displayed from the
cache. Otherwise, Emacs loads the image normally.

Function: **image-flush** spec &optional frame [¶](#index-image_002dflush)
:   This function removes the image with specification spec from the
    image cache of frame frame. Image specifications are compared
    using `equal`. If frame is `nil`, it defaults to the
    selected frame. If frame is `t`, the image is flushed on
    all existing frames.

    In Emacs’s current implementation, each graphical terminal possesses an
    image cache, which is shared by all the frames on that terminal
    (see [Multiple Terminals](#Multiple-Terminals)). Thus, refreshing an image in one frame
    also refreshes it in all other frames on the same terminal.

One use for `image-flush` is to tell Emacs about a change in an
image file. If an image specification contains a `:file`
property, the image is cached based on the file’s contents when the
image is first displayed. Even if the file subsequently changes,
Emacs continues displaying the old version of the image. Calling
`image-flush` flushes the image from the cache, forcing Emacs to
re-read the file the next time it needs to display that image.

Another use for `image-flush` is for memory conservation. If
your Lisp program creates a large number of temporary images over a
period much shorter than `image-cache-eviction-delay` (see
below), you can opt to flush unused images yourself, instead of
waiting for Emacs to do it automatically.

Function: **clear-image-cache** &optional filter animation-filter [¶](#index-clear_002dimage_002dcache)
:   This function clears an image cache, removing all the images stored in
    it. If filter is omitted or `nil`, it clears the cache for
    the selected frame. If filter is a frame, it clears the cache
    for that frame. If filter is `t`, all image caches are
    cleared. Otherwise, filter is taken to be a file name, and all
    images associated with that file name are removed from all image
    caches.

    This function also clears the image animation cache, which is a separate
    cache that Emacs maintains for animated multi-frame images
    (see [Multi-Frame Images](#Multi_002dFrame-Images)). If animation-filter is omitted or
    `nil`, it clears the animation cache in addition to the image
    caches selected by filter. Otherwise, this function removes the
    image with specification `eq` to animation-filter only from
    the animation cache, and does not clear any image caches. This can help
    reduce memory usage after an animation is stopped but the image is still
    displayed.

If an image in the image cache has not been displayed for a specified
period of time, Emacs removes it from the cache and frees the
associated memory.

Variable: **image-cache-eviction-delay** [¶](#index-image_002dcache_002deviction_002ddelay)
:   This variable specifies the number of seconds an image can remain in
    the cache without being displayed. When an image is not displayed for
    this length of time, Emacs removes it from the image cache.

    Under some circumstances, if the number of images in the cache grows
    too large, the actual eviction delay may be shorter than this.

    If the value is `nil`, Emacs does not remove images from the cache
    except when you explicitly clear it. This mode can be useful for
    debugging.

Function: **image-cache-size** [¶](#index-image_002dcache_002dsize)
:   This function returns the total size of the current image cache, in
    bytes. An image of size 200x100 with 24 bits per color will have a
    cache size of 60000 bytes, for instance.

---

Next: [Embedded Native Widgets](#Xwidgets), Previous: [Images](#Images), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.18 Icons [¶](#Icons-1)

Emacs sometimes uses buttons (for clicking on) or small graphics (to
illustrate something). Since Emacs is available on a wide variety of
systems with different capabilities, and users have different
preferences, Emacs provides a facility to handle this in a convenient
way, allowing customization, graceful degradation, accessibility, as
well as themability: *Icons*.

The central macro here is `define-icon`, and here’s a simple
example:

```
(define-icon outline-open button
  '((image "right.svg" "open.xpm" "open.pbm" :height line)
    (emoji "▶️")
    (symbol "▶" "➤")
    (text "open" :face icon-button))
  "Icon used for buttons for opening a section in outline buffers."
  :version "29.1"
  :help-echo "Open this section")
```

Which alternative will actually be displayed depends on the value of
the user option `icon-preference` (see [Icons](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Icons) in The GNU
Emacs Manual) and on the results of run-time checks for what the
current frame’s terminal can actually display.

The macro in the example above defines `outline-open` as an icon,
and inherits properties from the icon called `button` (so this is
meant as a clickable button to be inserted in a buffer). It is
followed by a list of *icon types* along with the actual icon
shapes themselves. In addition, there’s a doc string and various
keywords that contain additional information and properties.

To instantiate an icon, you use `icon-string`, which will
consult the current Customize theming, and the `icon-preference`
user option, and finally what the Emacs is able to actually display.
If `icon-preference` is `(image emoji symbol text)` (i.e.,
allowing all of these forms of icons), in this case,
`icon-string` will first check that Emacs is able to display
images at all, and then whether it has support for each of those
different image formats. If that fails, Emacs will check whether
Emacs can display emojis (in the current frame). If that fails, it’ll
check whether it can display the symbol in question. If that fails,
it’ll use the plain text version.

For instance, if `icon-preference` doesn’t contain `image`
or `emoji`, it’ll skip those entries.

Code can confidently call `icon-string` in all circumstances and
be sure that something readable will appear on the screen, no
matter whether the user is on a graphical terminal or a text terminal,
and no matter which features Emacs was built with.

Macro: **define-icon** name parent specs doc &rest keywords [¶](#index-define_002dicon)
:   Define an icon name, a symbol, with the display alternatives in
    spec, that can be later instantiated using `icon-string`.
    The name is the name of the resulting keyword.

    The resulting icon will inherit specs from parent, and from
    their parent’s parents, and so on, and the lowest descendent element
    wins.

    specs is a list of icon specifications. The first element of each
    specification is the type, and the rest is something that can be used
    as an icon of that type, and then optionally followed by a keyword
    list. The following icon types are available:

    `image`
    :   In this case, there may be many images listed as candidates. Emacs
        will choose the first one that the current Emacs instance can show.
        If an image is listed is an absolute file name, it’s used as is, but it’s
        otherwise looked up in the list `image-load-path`
        (see [Defining Images](#Defining-Images)).

    `emoji`
    :   This should be a (possibly colorful) emoji.

    `symbol`
    :   This should be a (monochrome) symbol character.

    `text`
    :   Icons should also have a textual fallback. This can also be used for
        the visually impaired: if `icon-preference` is just
        `(text)`, all icons will be replaced by text.

    Various keywords may follow the list of icon specifications. For
    instance:

    ```
    (symbol "▶" "➤" :face icon-button)
    ```

    Unknown keywords are ignored. The following keywords are allowed:

    `:face`
    :   The face to be used for the icon.

    `:height`
    :   This is only valid for `image` icons, and can be either a number
        (which specifies the height in pixels), or the symbol `line`,
        which will use the default line height in the currently selected
        window.

    `:width`
    :   This is only valid for `image` icons, and can be either a number
        (which specifies the width in pixels), or the symbol `font`,
        which will use the width in pixels of the current buffer’s default
        face font.

    doc should be a doc string.

    keywords is a list of keyword/value pairs. The following
    keywords are allowed:

    `:version`
    :   The (approximate) Emacs version this button first appeared. (This
        keyword is mandatory.)

    `:group`
    :   The customization group this icon belongs in. If not present, it is
        inferred.

    `:help-echo`
    :   The help string shown when hovering over the icon with the mouse
        pointer.

Function: **icon-string** icon [¶](#index-icon_002dstring)
:   This function returns a string suitable for display in the current
    buffer for icon.

Function: **icon-elements** icon [¶](#index-icon_002delements)
:   Alternatively, you can get a “deconstructed” version of icon
    with this function. It returns a plist (see [Property Lists](#Property-Lists)) where
    the keys are `string`, `face` and image. (The latter
    is only present if the icon is represented by an image.) This can be
    useful if the icon isn’t to be inserted directly in the buffer, but
    needs some sort of pre-processing first.

Icons can be customized with `M-x customize-icon`. Themes can
specify changes to icons with, for instance:

```
(custom-theme-set-icons
  'my-theme
  '(outline-open ((image :height 100)
                  (text " OPEN ")))
  '(outline-close ((image :height 100)
                   (text " CLOSE " :face warning))))
```

---

Next: [Buttons](#Buttons), Previous: [Icons](#Icons), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.19 Embedded Native Widgets [¶](#Embedded-Native-Widgets)

Emacs is able to display native widgets, such as GTK+ WebKit widgets,
in Emacs buffers when it was built with the necessary support
libraries and is running on a graphical terminal. To test whether
Emacs supports display of embedded widgets, check that the
`xwidget-internal` feature is available (see [Features](#Named-Features)).

To display an embedded widget in a buffer, you must first create an
xwidget object, and then use that object as the display specifier
in a `display` text or overlay property (see [The `display` Property](#Display-Property)).

Embedded widgets can send events notifying Lisp code about changes
occurring within them. (see [Xwidget events](#Xwidget-Events)).

Function: **make-xwidget** type title width height arguments &optional buffer related [¶](#index-make_002dxwidget)
:   This creates and returns an xwidget object. If
    buffer is omitted or `nil`, it defaults to the current
    buffer. If buffer names a buffer that doesn’t exist, it will be
    created. The type identifies the type of the xwidget component,
    it can be one of the following:

    `webkit`
    :   The WebKit component.

    The width and height arguments specify the widget size in
    pixels, and title, a string, specifies its title. related
    is used internally by the WebKit widget, and specifies another WebKit
    widget that the newly created widget should share settings and
    subprocesses with.

    The xwidget that is returned will be killed alongside its buffer
    (see [Killing Buffers](#Killing-Buffers)). You can also kill it using
    `kill-xwidget`. Once it is killed, the xwidget may continue to
    exist as a Lisp object and act as a `display` property until all
    references to it are gone, but most actions that can be performed on
    live xwidgets will no longer be available.

Function: **xwidgetp** object [¶](#index-xwidgetp)
:   This function returns `t` if object is an xwidget,
    `nil` otherwise.

Function: **xwidget-live-p** object [¶](#index-xwidget_002dlive_002dp)
:   This function returns `t` if object is an xwidget that
    hasn’t been killed, and `nil` otherwise.

Function: **kill-xwidget** xwidget [¶](#index-kill_002dxwidget)
:   This function kills xwidget, by removing it from its buffer and
    releasing window system resources it holds.

Function: **xwidget-plist** xwidget [¶](#index-xwidget_002dplist)
:   This function returns the property list of xwidget.

Function: **set-xwidget-plist** xwidget plist [¶](#index-set_002dxwidget_002dplist)
:   This function replaces the property list of xwidget with a new
    property list given by plist.

Function: **xwidget-buffer** xwidget [¶](#index-xwidget_002dbuffer)
:   This function returns the buffer of xwidget. If xwidget
    has been killed, it returns `nil`.

Function: **set-xwidget-buffer** xwidget buffer [¶](#index-set_002dxwidget_002dbuffer)
:   This function sets the buffer of xwidget to buffer.

Function: **get-buffer-xwidgets** buffer [¶](#index-get_002dbuffer_002dxwidgets)
:   This function returns a list of xwidget objects associated with the
    buffer, which can be specified as a buffer object or a name of
    an existing buffer, a string. The value is `nil` if buffer
    contains no xwidgets.

Function: **xwidget-webkit-goto-uri** xwidget uri [¶](#index-xwidget_002dwebkit_002dgoto_002duri)
:   This function browses the specified uri in the given
    xwidget. The uri is a string that specifies the name of a
    file or a URL.

Function: **xwidget-webkit-execute-script** xwidget script [¶](#index-xwidget_002dwebkit_002dexecute_002dscript)
:   This function causes the browser widget specified by xwidget to
    execute the specified JavaScript `script`.

Function: **xwidget-webkit-title** xwidget [¶](#index-xwidget_002dwebkit_002dtitle)
:   This function returns the title of xwidget as a string.

Function: **xwidget-resize** xwidget width height [¶](#index-xwidget_002dresize)
:   This function resizes the specified xwidget to the size
    widthxheight pixels.

Function: **xwidget-size-request** xwidget [¶](#index-xwidget_002dsize_002drequest)
:   This function returns the desired size of xwidget as a list of
    the form `(width height)`. The dimensions are in
    pixels.

Function: **xwidget-info** xwidget [¶](#index-xwidget_002dinfo)
:   This function returns the attributes of xwidget as a vector of
    the form `[type title width height]`.
    The attributes are usually determined by `make-xwidget` when the
    xwidget is created.

Function: **set-xwidget-query-on-exit-flag** xwidget flag [¶](#index-set_002dxwidget_002dquery_002don_002dexit_002dflag)
:   This function allows you to arrange that Emacs will ask the user for
    confirmation before exiting or before killing a buffer that has
    xwidget associated with it. If flag is non-`nil`,
    Emacs will query the user, otherwise it will not.

Function: **xwidget-query-on-exit-flag** xwidget [¶](#index-xwidget_002dquery_002don_002dexit_002dflag)
:   This function returns the current setting of xwidgets
    query-on-exit flag, either `t` or `nil`.

Function: **xwidget-perform-lispy-event** xwidget event frame [¶](#index-xwidget_002dperform_002dlispy_002devent)
:   Send an input event event to xwidget. The precise action
    performed is platform-specific. See [Input Events](#Input-Events).

    You can optionally pass the frame on which the event was generated via
    frame. On X11, modifier keys in key events will not be
    considered if frame is `nil`, and the selected frame is not
    an X-Windows frame.

    On GTK, only keyboard and function key events are supported. Mouse,
    motion, and click events are dispatched to the xwidget without going
    through Lisp code, and as such shouldn’t require this function to be
    called.

Function: **xwidget-webkit-search** query xwidget &optional case-insensitive backwards wrap-around [¶](#index-xwidget_002dwebkit_002dsearch)
:   Start an incremental search on the WebKit widget xwidget with
    the string query as the query. case-insensitive denotes
    whether or not the search is case-insensitive, backwards
    determines if the search is performed backwards towards the start of
    the document, and wrap-around determines whether or not the
    search terminates at the end of the document.

    If the function is called while a search query is already present,
    then the query specified here will replace the existing query.

    To stop a search query, use `xwidget-webkit-finish-search`.

Function: **xwidget-webkit-next-result** xwidget [¶](#index-xwidget_002dwebkit_002dnext_002dresult)
:   Display the next search result in xwidget. This function will
    signal an error if a search query has not been already started in
    xwidget through `xwidget-webkit-search`.

    If `wrap-around` was non-`nil` when `xwidget-webkit-search`
    was called, then the search will restart from the beginning of the
    document when its end is reached.

Function: **xwidget-webkit-previous-result** xwidget [¶](#index-xwidget_002dwebkit_002dprevious_002dresult)
:   Display the previous search result in xwidget. This function
    signals an error if a search query has not been already started in
    xwidget through `xwidget-webkit-search`.

    If `wrap-around` was non-`nil` when `xwidget-webkit-search`
    was called, then the search will restart from the end of the
    document when its beginning is reached.

Function: **xwidget-webkit-finish-search** xwidget [¶](#index-xwidget_002dwebkit_002dfinish_002dsearch)
:   Finish a search operation started with `xwidget-webkit-search` in
    xwidget. If there is no query currently ongoing, this function
    signals an error.

Function: **xwidget-webkit-load-html** xwidget text &optional base-uri [¶](#index-xwidget_002dwebkit_002dload_002dhtml)
:   Load text, a string, into xwidget, which should be a
    WebKit xwidget. Any HTML markup in text will be processed
    by xwidget while rendering the text.

    Optional argument base-uri, which should be a string, specifies
    the absolute location of the web resources referenced by text,
    to be used for resolving relative links in text.

Function: **xwidget-webkit-goto-history** xwidget rel-pos [¶](#index-xwidget_002dwebkit_002dgoto_002dhistory)
:   Make xwidget, a WebKit widget, load the rel-posth element
    in its navigation history.

    If rel-pos is zero, the current page will be reloaded instead.

Function: **xwidget-webkit-back-forward-list** xwidget &optional limit [¶](#index-xwidget_002dwebkit_002dback_002dforward_002dlist)
:   Return the navigation history of xwidget, up to limit
    items in each direction. If not specified, limit defaults to
    50.

    The returned value is a list of the form `(back here forward)`, where here is the current
    navigation item, while back is a list of items containing the
    items recorded by WebKit before the current navigation item, and
    forward is a list of items recorded after the current navigation
    item. back, here and forward can all be `nil`.

    When here is `nil`, it means that no items have been
    recorded yet; if back or forward are `nil`, it means
    that there is no history recorded before or after the current item
    respectively.

    Navigation items are themselves lists of the form `(idx title uri)`. In these lists, idx is an index that
    can be passed to `xwidget-webkit-goto-history`, title is
    the human-readable title of the item, and uri is the URI of the
    item. The user should normally have no reason to load uri
    manually to reach a specific history item. Instead, idx should
    be passed as an index to `xwidget-webkit-goto-history`.

Function: **xwidget-webkit-estimated-load-progress** xwidget [¶](#index-xwidget_002dwebkit_002destimated_002dload_002dprogress)
:   Return an estimate of how much data is remaining to be transferred
    before the page displayed by the WebKit widget xwidget is fully
    loaded.

    The value returned is a float ranging between 0.0 and 1.0.

Function: **xwidget-webkit-set-cookie-storage-file** xwidget file [¶](#index-xwidget_002dwebkit_002dset_002dcookie_002dstorage_002dfile)
:   Make the WebKit widget xwidget store cookies in file.

    file must be an absolute file name. The new setting will also
    affect any xwidget that was created with xwidget as the
    `related` argument to `make-xwidget`, and widgets related to
    those as well.

    If this function is not called at least once on xwidget or a
    related widget, xwidget will not store cookies on disk at all.

Function: **xwidget-webkit-stop-loading** xwidget [¶](#index-xwidget_002dwebkit_002dstop_002dloading)
:   Terminate any data transfer still in progress in the WebKit widget
    xwidget as part of a page-loading operation. If a page is not
    being loaded, this function does nothing.

---

Next: [Abstract Display](#Abstract-Display), Previous: [Embedded Native Widgets](#Xwidgets), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
