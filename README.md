Image offset calculation tool
=============================
This program in Python uses phase correlation to calculate translation
of images in a stack relative to a single anchor image.

It does not modify the images in any way. Only the registration is
done, which reveals the offsets between the images and anchor. It's on
us to decide what we do with that information afterward...

Dependencies
------------
* Modern Python version (>=3.12).
* `numpy` as math engine.
* `tifffile` with `imagecodecs` to read compressed TIFFs.

Install them with `pip install -r requirements.txt`, or any package
manager you like. Best practice is to do that in an independent Python
virtual environment (venv).

No other, usually heavyweight, image processing packages needed!

Usage
-----
For the script usage and parameters see `python img_offset -h`.

The usage is pretty straightforward, so only some special notes:

_Hanning window_ is a mask (it's sort of a vignette) that will be
applied to all images. In the frequency domain, the opposite image
borders are connected, building a torus (or a donut if you like).
By applying it, we make the border connections continuous. This
should lead to more precise results.

_Gaussian preblur_ can be used to smooth out the fine noise, which
could fool the phase correlation algorithm. As we are already in the
frequency domain, we use the convolution theorem and apply the blur
very effectively.

How to interpret the results
----------------------------
For every image, except the anchor, we get three numbers as a result.

The first two are the found X and Y offsets. These resulting numbers
mean how to shift that particular image to align it with the anchor.
Negative correction means moving left/up and positive right/down.

The third number is the quality indicator, which tells us how good
the match is. In an ideal world it should be `1.0`. Practically,
you will see something very close to this ideal for a good match.

Limitations
-----------
* The script accepts only single-layer TIFF images.

* All input images must have the exact same size.

* Minimal Gaussian sigma is 0.1px. If it's lower, then the whole blur
  argument is ignored.

License
-------
BSD-3-Clause license. See `LICENSE.txt` for full text.
