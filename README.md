# pysten-cup
A simple steganographic encoder / decoder in Python

# How does it work?

## Encoding
Let's start with one pixel.

Each pixel is made of 3 color components: red, green, and blue.

Each of these components has a brightness value, which ranges from 0 to 255.

This means that each pixel is stored as 3 bytes.

When using images to hide things, we focus on only the last two bits in each component.

Changing these bits hardly changes the color at all, but the data is still stored.

This means we can hide literally anything digital in it. Usually, another image.

## Image Color Depth / Normalization
Hiding an image inside another image requires that the 8 bits of each component be brought down to just 2.

This is fairly simple to do by integer dividing the component by 64.

This has the unfortunate side effect of decimating the color depth of the image.

Usually, one pixel can display a range of 256^3 colors, which works out to about 16.7 million.

Compressing it to two means each pixel can only display 4^3 colors, which works out to 64.

Though this usually causes the image to look like crap, it's often a worthwhile tradeoff to hide it.

The resulting image has pixels whose component values range from 0-3. Displaying pixels with these values will result 
in an extremely dark image. 

To fix this, the pixels need to be normalized. 

In practice, this just means multiplying each component by 85.

This brings 3 back to its maximum value of 255, and adjusts all other colors accordingly.

# Usage
## Decoding
Using this program requires two PNG images as input, the second one is encoded into (or hidden in) the first one.

To encode an image, use the -e flag, followed by the public image, then the private image.

```python3 pystencup.py -e public.png private.png```

By default, the resulting output will be named `output.png`. To change this, use the -o flag.

```python3 pystencup.py -e public.png private.png -o hiddenimage.png```

(**Note**: the program will always output as a PNG, so adding .png to the end of the output name is optional)

Finally, to render a preview of the renormalized image, use the -p flag. By default, the name of the preview image will be the name of the output with "_preview" appended. To change this, specify a filename after the flag.

```python3 pystencup.py -e public.png private.png -o hiddenimage.png -p hiddenpreview.png```

(**Note**: again, the .png on the end is optional)

## Decoding
To decode an image, use the -d flag, followed by the name of the image to decode.

```python3 pystencup.py -d hiddenimage.png```

By default, the decoded image will be the name of the encoded image with "_decoded" appended. To change this, use the -o flag.

```python3 pystencup.py -d hiddenimage.png -o hiddendecode.png```

By default, the decoded image will automatically be normalized. To render the output as a non-normalized image, use the -n tag. 

```python3 pystencup.py -d hiddenimage.png -o hiddendecode.png -n```