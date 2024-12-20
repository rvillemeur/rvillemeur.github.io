---
layout: post
title: How form are managed
permalink: blog/form
date: '2021-02-06 21:15:55 -0500'
categories: pharo gtoolkit gui morphic form
comments_id: 3
draft: false
---

## Note on Form and diplay color

A bitmap image is a raster image (containing pixel data as opposed to vector
images) format. Each pixel of a bitmap image is defined by a single bit or a
group of bits. Hence, it is called the bitmap or a map of bits and pixels.

A Bitmap image is an uncompressed file format which means, every pixel of an
image has its own bit (or group of bits) in the file.

In Pharo, a Form is a rectangular array of pixels, used for holding images.
All pictures, including character images are Forms.

Creation of a Form:

```smalltalk
Form extent: extentPoint depth: bitsPerPixel fromArray: anArray offset: offsetPoint

extent: the number of point composing the form.
depth: the color depth of the form
array: data of the form.
offset: Specify an offset to move the form from coordinate 0@0 when combining form.
```

The depth of a Form is how many bits are used to specify the color at each pixel.
The supported depths (in bits) are 1, 2, 4, 8, 16, and 32. The color depth is a
measure of an individual image pixel to accurately represent a color. Color
depth is calculated in bits-per-pixel or bpp.

For example, 1-bit color depth or 1bpp means a pixel can have a 1-bit color or 2
values. Monochromatic images have 1-bit color depth because a pixel can be true
black or true white.

The number of actual colors at these depths are: 2, 4, 16, 256, 32768, and 16 million.
For different color depth, you can store a different number of pixel for each
element compising the array of data.

|Depth   | number of color  | pixel displayed |
|--------|------------------|-----------------|
|1       |2^1 = 2           | 32              |
|2       |2^2 = 4           | 16              |
|4       |2^4 = 16          | 8               |
|8       |2^8 = 256         | 4               |
|16      |2^16 = 32768      | 2               |
|32      |2^32 = 16 millions| 1               |

The bits representing the bitmap pixels are packed in rows. The size of each row
is rounded up to a multiple of 4 bytes (32 bits). The pixel array describes the
image pixel by pixel. You can also have an alpha-channel to add transparency in
the image using 32-bit color depth.

The coordinates of a Form start from the top-left corner much like most graphic
system out there (why ? Because Western language are mostly written left to right,
top to bottom, and initial text display follow this convention, instead of
classic cartesian coordinate). Forms are indexed starting at 0 instead of 1;
thus, the top-left pixel of a Form has coordinates 0@0.

The actual bits are held in a Bitmap, whose internal structure is different at
each depth. Class Color allows you to deal with colors without knowing how they
are actually encoded inside a Bitmap.

You also have indexed color. For each pixel, you have a color index that will
provide its true color. This is defined as *ColorForm* is Pharo
In the Form, the bitmap is an index to a color map index, which size is $2^{depth}$
color

```smalltalk
ColorForm extent: 32@2 depth: 8
```

+-+-+-+-+    |index  | red   | green | blue  |
| | | | |    |-------|-------|-------|-------|
+-+-+-+-+ -> | 0     |       |       |       |
| | | | |    | ...   |0 - 255|0 - 255|0 - 255|
+-+-+-+-+    |2^depth|       |       |       |

Through the color map, you can access different color than the default defined
in the color depth. Furthermore, Color can be be defined with alpha channel.

## Color depth in action

To construct an image, we need to know how many bits or byte an individual pixel
contains in the Pixel Data.

### 1 bit depth

you can define the color of 32 pixel with color depth 1
playing with color and form.

```smalltalk
(Form extent: 32@2 "size of the form"
 depth: 1 "color depth")
 initFromArray: #( "pixel color, expressed on 4 bytes"
        2r10011000000100010000000000000001
        2r11000000000000000000000000000000
)
; magnifyBy: 10.
```

```smalltalk
(Form extent: 32@2 "size of the form"
 depth: 1 "color depth - 16 => 2 bits to define color value")
 initFromArray: #(
 2r10101010101010101010101010101010
 2r01010101010101010101010101010101 "2 colors"
)
; magnifyBy: 25.
```

Use ColorForm if you want to use color other than black and white:

```smalltalk
|pict map|
pict := ColorForm extent: 32@2 depth: 1.

"create a color map of 2^depth color"
map := {  Color r: 0.0 g: 0.5992179863147605 b: 0.19941348973607037 alpha: 1.0.
            Color blue.
 }.
pict colors: map.
pict initFromArray: #(
2r01010101010101010101010101010101
2r10101010101010101010101010101010).
pict magnifyBy: 25
```

This is how the normal cursor is defined. By default, a cursor size can only
be 16@16. However, because data is stored on 32 bits, we need to add 16
additional bit as white to store it, even if they wont be displayed.

```smalltalk
(Form extent: 16@16
    depth: 1
    fromArray: #(
        2r10000001000000000000000000000000
        2r11000000000000000000000000000000
        2r11100000000000000000000000000000
        2r11110000000000000000000000000000
        2r11111000000000000000000000000000
        2r11111100000000000000000000000000
        2r11111110000000000000000000000000
        2r11111000000000000000000000000000
        2r11111000000000000000000000000000
        2r10011000000000000000000000000000
        2r00001100000000000000000000000000
        2r00001100000000000000000000000000
        2r00000110000000000000000000000000
        2r00000110000000000000000000000000
        2r00000011000000000000000000000000
        2r00000011000000000000000000000000)
    offset: 0@0) magnifyBy: 10.
```

### 2 bits depth

```smalltalk
(Form extent: 16@1 "size of the form"
 depth: 2 "color depth - 16 => 2 bits to define color value")
 initFromArray: #( "pixel color, expressed on 4 bytes or 32 bits, which is the maximum of color depth"
"[ 0000 0000 0000 0000 0000 0000 0000 0000]
    first   second    third    fourth pixel  
  Each pixel can have 2^2 = 4 different color"
"[ 0000 0000 0000 0000 0000 0000 0000 0000]"
 2r00011011000110110001101100011011 "4 colors"
)
; magnifyBy: 25.
```

With ColorForm:

```smalltalk
|pict map|
pict := ColorForm extent: 16@1 depth: 2.

"create a color map of 2^depth color"
map := {  Color white.
            Color r: 0.0 g: 0.5992179863147605 b: 0.19941348973607037 alpha: 1.0.
            Color blue.
            Color red.
 }.
pict colors: map.
pict initFromArray: #(2r00011011000110110001101100011011).
pict magnifyBy: 25
```

### 4 bits depth

you can define the color of 8 pixel with color depth 16

```smalltalk
(Form extent: 8@2 "size of the form"
 depth: 4 "color depth - 16 => 2 bits to define color value")
 initFromArray: #( "pixel color, expressed on 4 bytes or 32 bits, which is the maximum of color depth"
"[ 0000 0000 0000 0000 0000 0000 0000 0000]
    first   second    third    fourth pixel  
  Each pixel can have 2^4 = 16 different color"
"[ 0000 0000 0000 0000 0000 0000 0000 0000]"
 2r00000001001000110100010101100111 "8 colors"
 2r10001001101010111100110111101111 "8 colors"
)
; magnifyBy: 25.
```

With ColorForm:

```smalltalk
|pict map|
pict := ColorForm extent: 8@1 depth: 4.

"create a color map of 2^depth color"
map := {  Color transparent.
            Color white.
            Color r: 0.0 g: 0.5992179863147605 b: 0.19941348973607037 alpha: 1.0.
            Color gray.
            Color red.
            Color green.
            Color blue.
            Color yellow.
            Color transparent.
            Color white.
            Color blue.
            Color gray.
            Color red.
            Color green.
            Color blue.
            Color yellow.
 }.
pict colors: map.
pict initFromArray: #(2r01000010001100011011110111101111).
pict magnifyBy: 25
```

### 8 bits depth

"you can define the color of 4 pixel with color depth 8"

```smalltalk
(Form extent: 4@64 "size of the form"
 depth: 8 "color depth - 16 => 2 bits to define color value")
 initFromArray: #( "pixel color, expressed on 4 bytes or 32 bits, which is the maximum of color depth"
"[ 00000000 00000000 00000000 00000000]
    first   second    third    fourth pixel  
  Each pixel can have 2^8 = 256 different color"
"[ 00000000 00000000 00000000 00000000]"
2r00000000010000001000000011000000
2r00000001010000011000000111000001
2r00000010010000101000001011000010
2r00000011010000111000001111000011
2r00000100010001001000010011000100
2r00000101010001011000010111000101
2r00000110010001101000011011000110
2r00000111010001111000011111000111
2r00001000010010001000100011001000
2r00001001010010011000100111001001
2r00001010010010101000101011001010
2r00001011010010111000101111001011
2r00001100010011001000110011001100
2r00001101010011011000110111001101
2r00001110010011101000111011001110
2r00001111010011111000111111001111
2r00010000010100001001000011010000
2r00010001010100011001000111010001
2r00010010010100101001001011010010
2r00010011010100111001001111010011
2r00010100010101001001010011010100
2r00010101010101011001010111010101
2r00010110010101101001011011010110
2r00010111010101111001011111010111
2r00011000010110001001100011011000
2r00011001010110011001100111011001
2r00011010010110101001101011011010
2r00011011010110111001101111011011
2r00011100010111001001110011011100
2r00011101010111011001110111011101
2r00011110010111101001111011011110
2r00011111010111111001111111011111
2r00100000011000001010000011100000
2r00100001011000011010000111100001
2r00100010011000101010001011100010
2r00100011011000111010001111100011
2r00100100011001001010010011100100
2r00100101011001011010010111100101
2r00100110011001101010011011100110
2r00100111011001111010011111100111
2r00101000011010001010100011101000
2r00101001011010011010100111101001
2r00101010011010101010101011101010
2r00101011011010111010101111101011
2r00101100011011001010110011101100
2r00101101011011011010110111101101
2r00101110011011101010111011101110
2r00101111011011111010111111101111
2r00110000011100001011000011110000
2r00110001011100011011000111110001
2r00110010011100101011001011110010
2r00110011011100111011001111110011
2r00110100011101001011010011110100
2r00110101011101011011010111110101
2r00110110011101101011011011110110
2r00110111011101111011011111110111
2r00111000011110001011100011111000
2r00111001011110011011100111111001
2r00111010011110101011101011111010
2r00111011011110111011101111111011
2r00111100011111001011110011111100
2r00111101011111011011110111111101
2r00111110011111101011111011111110
2r00111111011111111011111111111111
); magnifyBy: 25
```

Using ColorForm to reverse the color:

```smalltalk
|pict map|
pict := ColorForm extent: 4@64 depth: 8.

"create a color map of 2^depth color"
map := (Color indexedColors copy) reverse .
map at: 1 put: Color transparent.
pict colors: map.


pict initFromArray: #(
2r00000000010000001000000011000000
2r00000001010000011000000111000001
2r00000010010000101000001011000010
2r00000011010000111000001111000011
2r00000100010001001000010011000100
2r00000101010001011000010111000101
2r00000110010001101000011011000110
2r00000111010001111000011111000111
2r00001000010010001000100011001000
2r00001001010010011000100111001001
2r00001010010010101000101011001010
2r00001011010010111000101111001011
2r00001100010011001000110011001100
2r00001101010011011000110111001101
2r00001110010011101000111011001110
2r00001111010011111000111111001111
2r00010000010100001001000011010000
2r00010001010100011001000111010001
2r00010010010100101001001011010010
2r00010011010100111001001111010011
2r00010100010101001001010011010100
2r00010101010101011001010111010101
2r00010110010101101001011011010110
2r00010111010101111001011111010111
2r00011000010110001001100011011000
2r00011001010110011001100111011001
2r00011010010110101001101011011010
2r00011011010110111001101111011011
2r00011100010111001001110011011100
2r00011101010111011001110111011101
2r00011110010111101001111011011110
2r00011111010111111001111111011111
2r00100000011000001010000011100000
2r00100001011000011010000111100001
2r00100010011000101010001011100010
2r00100011011000111010001111100011
2r00100100011001001010010011100100
2r00100101011001011010010111100101
2r00100110011001101010011011100110
2r00100111011001111010011111100111
2r00101000011010001010100011101000
2r00101001011010011010100111101001
2r00101010011010101010101011101010
2r00101011011010111010101111101011
2r00101100011011001010110011101100
2r00101101011011011010110111101101
2r00101110011011101010111011101110
2r00101111011011111010111111101111
2r00110000011100001011000011110000
2r00110001011100011011000111110001
2r00110010011100101011001011110010
2r00110011011100111011001111110011
2r00110100011101001011010011110100
2r00110101011101011011010111110101
2r00110110011101101011011011110110
2r00110111011101111011011111110111
2r00111000011110001011100011111000
2r00111001011110011011100111111001
2r00111010011110101011101011111010
2r00111011011110111011101111111011
2r00111100011111001011110011111100
2r00111101011111011011110111111101
2r00111110011111101011111011111110
2r00111111011111111011111111111111
); magnifyBy: 25
```

### 16 bit depth

"you can define the color of 2 pixel with color depth 16"

```smalltalk
(Form extent: 6@1 "size of the form"
 depth: 16 "color depth - 16 => 2 bits to define color value 65K color possible")
 initFromArray: #( "pixel color, expressed on 4 bytes or 32 bits, which is the maximum of color depth"
"[ 0  XXXXX XXXXX XXXXX ]

----- ----- -----

​       R     G     B
​     first pixel         second pixel   "
​        2r01111100000000000000000000011111 "red - blue"
​        2r00000011111000000111111111100000 "green -yellow"
​        2r01111111111111111000000000000000 "white -black"
)
; magnifyBy: 25.
```

### 32 bits depths

For 24 and 32 bits RBG, there is 8 bits allocated to each color component. In
32 bits, there is an additional alpha channel, to manage transparency.
In RGB space any colour is represented as a point inside a colour cube with
orthogonal axes r,g,b. Grey values form a straight line from black to white
along the diagonal of the cube, r = g = b.

```txt
           cyan (0,1,1)+--------------------------+ White (1,1,1)
                      /|                         /|
                     / |                        / |
                    /  |                       /  |
                   /   |                      /   |
                  /    |                     /    |
    Blue (0,0,1) +--------------------------+ Magenta(1,0,1)
                 |     |                    |     |
                 |     |                    |     |
                 |Green+--------------------|-----+ yellow (1,1,0)
                 |    /(0,1,0)              |    /
          `      |   /                      |   /
                 |  /                       |  /
                 | /                        | /
                 |/                         |/
    Black (0,0,0)+--------------------------+ red(1,0,0)
```

```smalltalk
(Form extent: 7@1 "size of the form"
 depth: 32 "color depth")
 initFromArray: #( "pixel color, expressed on 4 bytes or 32 bits, which is the maximum of color depth"
"colore are:
2r 00000000 00000000 00000000 00000000
     ALPHA    RED         BLUE     GREEN   "
        16rffff0000 "red"
        16rff0000ff "blue"
        16rff00ff00 "green"
        16rff000000 "black"
        16rffffffff "white"
        16rf0ff00d9 "pink"
        16r2fff0000 "red with opacity"
)
; magnifyBy: 15.
```

Data is stored in Byte - remember than 8 bit = 1 bytes
Each entry of the color palette takes 4 bytes to define a color

```smalltalk
(Form extent: 3@3 "size of the form"
 depth: 32 "color depth")
 initFromArray: #( "pixel color, expressed on 4 bytes or 32 bits, which is the maximum of color depth"
"colore are:
2r 00000000 00000000 00000000 00000000
     ALPHA    RED         BLUE     GREEN   "
        16rffff0000 "red"      16rff000000 "black" 16rff0000ff "blue"
        16rff000000 "black" 16rffffffff "white" 16rff000000 "black"
        16rff00ff00 "green" 16rff000000 "black" 16rffffff00 "yello"

)
; magnifyBy: 15.
```

**Note**
24 bits doesn't have alpha channel -> to be confirmed on pharo

## Library built on top of Form

[Image Form Library](https://github.com/pablo1n7/ImageForm)

[script to manipulate image](https://80738163270632.blogspot.com/2018/10/pharo-script-of-day-proto-proto-image.html)

## Reference

[Bitmap format explanation](http://paulbourke.net/dataformats/bitmaps/)
