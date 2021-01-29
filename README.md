# Tensortown (working title)

A 2021 update to original work by [Tensorflow](https://www.tensorflow.org) and
[Douglas Duhaime](http://github.com/duhaime) for classifying images
using the Inception framework.

Mr. Duhaime published his work in 2017
as [Identifying Similar Images with Tensorflow](https://douglasduhaime.com/posts/identifying-similar-images-with-tensorflow.html). The original Tensorflow work
is floating around the internet in various forms.

My project is built on these original works but doesn't directly fork them.
Find it on [GitHub](https://github.com/pootriarch/tensortown.git).

## Components

### classify_image.py

This adaptation of classify_image.py updates and merges the versions by Google and
The Tensorflow Authors that can be seen across the internet, and adopts vector
calculation code from Mr. Duhaime.

Updates to original:

+ Updates deprecated Tensorflow calls
+ Adopts Mr. Duhaime's change to allow batch image processing
+ Changes main() to accept direct filename arguments instead of `--image_file`
+ Corrects imports and usage for collections
+ Adds pprint() of dict results at end of run

## Motivation

This is the first step in a vaporware dog picker app, intended to be a mechanism for identifying
the breed of a dog that you can see in your head, but don't have an exact photo of.
The mechanism would be similar to a vision exam, where you're shown a picture of a dog and
asked to pick one of three neighbors, ad infinitum, with matching pooch information shown
along the way.

## Requirements

This project is built using Anaconda, Tensorflow, and Python 3.6, the latter being the
least common denominator of the Tensorflow tools.

The sequencing of these tools is important and non-obvious; see the
Getting Started section below.

+ [Anaconda](https://www.anaconda.com/)
+ `numpy` (via conda)
+ [Tensorflow](https://www.tensorflow.org) (via conda)
+ `psutil` (via pip)
+ `annoy` (via pip)
+ `scipy` (via conda)
+ `nltk` (via conda)

## Getting Started

### Anaconda

Homebrew is the most straightforward way to install Anaconda on supported operating
systems, but because Anaconda installs a lot of things that could conflict with the
host, it has to be installed as a cask using `brew install --cask anaconda`.

Anaconda won't be accessible just sitting in a cask.
Once installed, find its location, which was printed during
installation, something like `/usr/local/anaconda3`.
Then run `conda init` to set it up. Be aware of these gotchas:

* `conda init` expects to write to files inside the top-level `.conda` folder
of your home directory, _but the installer may have created this as root_.
Be prepared to `sudo chmod -R` this directory back to your own account.

* `conda init` modifies your `.bash_profile` (or that of whichever shell you're
running). If Anaconda will be a side dalliance and you still code outside of
Anaconda, you'll need to move the conda block to another file that can be
read via `source`, or you'll need to use an IDE such as VSCode to [manage
which Python env you're using](https://code.visualstudio.com/docs/python/environments).

### Python

You'll need an Anaconda environment in which to encase your project. Make sure
you're starting from within Anaconda - your shell prompt will include `(base)`.
If it doesn't say that, see the above instructions for `conda init`.

Start out by running:

`(base) borg:~ emma$` **`conda create -n 3.6 python=3.6`**\
`(base) borg:~ emma$` **`conda activate 3.6`**\
`(3.6) borg:~ emma$`

You can of course name your environment anything by replacing the first instance of `3.6` in
each command above to a name of your choosing.

### Tensorflow

Now attempt to install numpy and tensorflow.

`(3.6) borg:~ emma$` **`conda install -c conda-forge numpy`**\
`(3.6) borg:~ emma$` **`conda install -c conda-forge tensorflow`**

If you clear that, you can try to import tensorflow.

`(3.6) borg:~ emma$` **`python`**\
`Python 3.6.12 |Anaconda, Inc.| (default, Sep  8 2020, 17:50:39)`\
`[GCC Clang 10.0.0 ] on darwin`\
`Type "help", "copyright", "credits" or "license" for more information.`\
`>>>` **`import tensorflow`**

Several things can go wrong along this path.

#### _Warning: (package) uses Python version 3.x, but you have 3.6_

The instructions I initially followed were to start with a Python 3.5 environment,
which would have been accurate when they were written. One of the conda installs
complained that it should have been 3.6 based on what it found inside
a package. By the time you read this, that could be yet a higher 3.x version.

You don't want to start off on your back foot, so tear down your environment
with `conda remove` and rebuild it with the suggested 3.x Python version.

If that version number says 4 (or higher), back away slowly and find someone else's
instructions. I can't help you.

#### _ImportError: numpy.core.multiarray failed to import_

This applies to `ImportError` messages that complain about
numpy after you attempt to import tensorflow (but not warnings -
for warnings such as `FutureWarning`, see below).

Your base operating system may have an installation of numpy, but it's too
old for tensorflow to use. First attempt to `conda install` numpy again inside
your 3.6 environment (and make sure you're really inside that environment).

If installing numpy before tensorflow doesn't work, there are things that
you can try, but there are risks; read up (elsewhere) before doing them.

* Uninstall numpy from your 3.6 environment, install it to the conda base
environment, and try again from 3.6. `conda deactivate` takes you out of
3.6 and back to base.

* Uninstall numpy from both 3.6 and conda base, then upgrade it in your
core operating system using `pip3 install numpy` - this may require sudo,
and if it does, understand that there may be collateral risks.

#### _FutureWarning: Passing (type, 1) or '1type' as a synonym of type is deprecated; in a future version of numpy, it will be understood as (type, (1,)) / '(1,)type'_

A `FutureWarning` is just that, a warning, and if you get this, Tensorflow should
run. Tensorflow 1.14.x triggers deprecation warnings in
numpy 1.17+, indicating that it needs to be updated to work with upcoming
versions of numpy.

[By the time you read this, Tensorflow 1.15 should be out and this problem
should be moot](https://github.com/tensorflow/tensorflow/issues/40500).

### psutil

_Validate the need for this._

### annoy/scipy/nltk

_Upcoming._

## Usage

### classify_image

Usage: `python classify_image.py [options] files`

* `num_top_predictions` - The number of predictions to be returned per image, default 5
* `vector_dir` - The directory in which to store vector info files, default `image_vectors`
* `vector_suffix` - The suffix to append to an image file name to form a vector info
  file name, default `.npz`
* `model_dir` as used in original code; see internal documentation

## Reference

+ [Identifying Similar Images with Tensorflow](https://douglasduhaime.com/posts/identifying-similar-images-with-tensorflow.html) - Douglas Duhaime, August 2017
+ [Rethinking the Inception Architecture for Computer Vision](https://arxiv.org/abs/1512.00567) - Christian Szegedy and Zbigniew Wojna _et al_, December 2015
+ [Tensorflow tutorial: Image classification](https://www.tensorflow.org/tutorials/images/classification)
+ [Stanford dogs dataset](http://vision.stanford.edu/aditya86/ImageNetDogs/) and
[Reference document](http://people.csail.mit.edu/khosla/papers/fgvc2011.pdf)

## Contributors

I am a lone wolf. But lift anything you like, within the parameters of the software
licenses.

## License

The original `classify_image.py` authors - listed as either Google or Tensorflow,
depending on where one finds the file -
licensed it under [Apache 2.0](http://opensource.org/licenses/Apache-2.0).
The version here adopts the Apache license.
It includes enhancements by Mr. Duhaime, which are implicitly covered under the
same license.

classify_vectors is directly derived from original work by Mr. Duhaime. It did not
carry original copyright information, so I assert that original copyright lies with
Mr. Duhaime, with his original and my modified files being covered by the
[MIT license](https://opensource.org/licenses/MIT). Mr. Duhaime is of course welcome
to demonstrate prior licensing, which would be adopted here.