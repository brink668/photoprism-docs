Many photographers keep their originals in some sort of lossless RAW format instead of compressed JPEG, especially when shooting with a Digital SLR. Some [mobile phones](https://www.fredericpaulussen.be/how-to-raw-photos-huawei-p30-pro/) also support RAW or use HEIC/HEIF for a similar purpose. PhotoPrism aims at providing excellent support for all [RAW](https://en.wikipedia.org/wiki/Raw_image_format) formats, independent of camera brand and model. Please let us know when there is an issue with your specific device.

Web browsers in general can not display RAW image files. They need to be converted, which is what our import and convert commands do. You'll also find a checkbox for this step in our Web UI.

In addition, PhotoPrism now also supports TIFF, PNG, BMP and GIF files. Be aware that files in those formats often don't contain useful metadata and are typically used for screenshots, charts, graphs and icons only.

![](img/editPhoto.png)

## Sips ##

On a Mac, PhotoPrism can convert multiple files at once using Sips (pre-installed on OS X). It is not available for other operating systems, as far as we know.

## Darktable ##

We've recently upgraded Darktable from 2.6.2 to 3.0.0. Since the old PPA is not maintained anymore, we switched to this repository that is hosted by SuSE:

https://software.opensuse.org/download.html?project=graphics:darktable:master&package=darktable

Note that PhotoPrism can only run one instance of `darktable-cli` because it uses a lock file. If you run it more than once, it will fail. Be prepared to wait a bit.

### Using Darktable as library ###

We had the idea to use [cgo](https://golang.org/cmd/cgo/) and link directly against `libdarktable.so` to convert RAW images to JPEG, see [darktable-dev mailing list](https://www.mail-archive.com/darktable-dev@lists.darktable.org/msg03608.html). However this requires calling/wrapping a large number of C functions and also has other [disadvantages](https://dave.cheney.net/2016/01/18/cgo-is-not-go). The idea is postponed until a clear benefit becomes visible. Running `darktable-cli` or any other binary that does the job (see above) seems to be the way to go.

## Comparison of RAW to JPEG converters ##

- [darktable](https://www.darktable.org/) - popular open-source photography app and raw developer; available for Mac, Linux, and Windows; supports XMP (compatible with photoshop/lightroom?)
- Mac OS X ships with [sips](https://coderwall.com/p/nhp7mq/convert-raw-photos-to-jpg-in-the-mac-os-terminal): `sips -s format jpeg IMAGE.RAW --out IMAGE.JPG`
- [Photivo](http://photivo.org/) - open-source photo processor; available for Mac, Linux, and Windows; no XMP support?
- [RawTherapee](https://rawtherapee.com/) - open-source RAW image processing app; available for Mac, Linux, and Windows; no XMP support?
- [digiKam](https://www.digikam.org/about/) - open-source digital photo management application based on Qt (KDE); available for Mac, Linux, and Windows; supports XMP (compatible with photoshop/lightroom?)
- [UFRaw](http://ufraw.sourceforge.net/) - Unidentified Flying Raw is a utility to read and manipulate raw images from digital cameras

Tool | Command line options| Compatible OS | JPG Diff* | XMP support | Possible settings | EXIF Diff* | Compatible with Raspberry (ARM64)
------------ | ------------- | ------------ | ------------- | ------------ | ------------ | ------------| ------------
[Darktable](https://www.darktable.org/usermanual/en/overview/) | 1) `darktable-cli IMG_0310.CR2  IMG_0310_darktable1.jpg` 2) `darktable-cli IMG_0310_EDITED.CR2 IMG_0310_EDITED.xmp IMG_0310_EDITED_darktable2.jpg` | macOS, Linux, Windows | **** | yes (but seems to be not compatible with adobe xmps) | ? | **** | [yes](https://launchpad.net/ubuntu/artful/arm64/darktable)
[Sips](https://ss64.com/osx/sips.html) |`sips -s format jpeg IMG_0310.CR2 --out IMG_0310_sips.jpg` | macOS | ***** | no | ? | **** | not available for ubuntu
[Rawtherapee](https://rawpedia.rawtherapee.com/Command-Line_Options) | `rawtherapee-cli -o IMG_0310_rawtherapee.jpg -c IMG_0310.CR2` | macOS, Linux, Windows | *** | no | ? | **** | [yes](https://launchpad.net/ubuntu/eoan/arm64/rawtherapee)
[UFraw](https://www.systutorials.com/docs/linux/man/1-ufraw/) | `ufraw-batch --out-type=jpg --output=IMG_0310_ufraw.jpg IMG_0310.CR2` | macOS, Linux, Windows| * | no | ? | ** | [yes](https://packages.ubuntu.com/search?keywords=ufraw)
[ImageMagick](https://www.imagemagick.org/script/command-line-processing.php) | `magick IMG_0310.CR2 IMG_0310_magick.jpg` | macOS, Linux, Windows | * | no | ? | ** | [yes](https://packages.ubuntu.com/search?keywords=imagemagick)
Digikam | ? | macOS, Linux, Windows | - | - | - | - | - | ? |
Photiva | ? | macOS, Linux, Windows | - | - | - | - | - | ? |
`*` Compared to JPG/EXIF converted from photoshop

### Image Diff ###
The following table shows the difference between the JPEG files converted by Darktable, Sips, Rawtherapee, UFraw and ImageMagick compared to Adobe Photoshop. Red are pixel that differ from the photoshop version, white are equal pixels. In total 5 Images have been compared ([full results](https://dl.photoprism.org/assets/img/raw-converter/)). The diff was created using https://github.com/ewanmellor/git-diff-image.

Tool         | Diff (left is Photoshop)                        |
------------ | ----------------------------------------------- |
Darktable    |  <img src="https://dl.photoprism.org/assets/img/raw-converter/ps_vs_darktable.jpg" width="512"><br><img src="https://dl.photoprism.org/assets/img/raw-converter/ps_vs_darktable_2.jpg" width="512"> |
[Sips](https://ss64.com/osx/sips.html) | <img src="https://dl.photoprism.org/assets/img/raw-converter/ps_vs_sips.jpg" width="512"><br><img src="https://dl.photoprism.org/assets/img/raw-converter/ps_vs_sips_2.jpg" width="512"> |
[Rawtherapee](https://rawpedia.rawtherapee.com/Command-Line_Options) | <img src="https://dl.photoprism.org/assets/img/raw-converter/ps_vs_rawtherapee.jpg" width="512"><br><img src="https://dl.photoprism.org/assets/img/raw-converter/ps_vs_rawtherapee_2.jpg" width="512"> |
[UFraw](https://www.systutorials.com/docs/linux/man/1-ufraw/) | <img src="https://dl.photoprism.org/assets/img/raw-converter/ps_vs_ufraw.jpg" width="512"> |
[ImageMagick](https://www.imagemagick.org/script/command-line-processing.php) | <img src="https://dl.photoprism.org/assets/img/raw-converter/ps_vs_imagemagick.jpg" width="512"> |

### EXIF Diff ###
The following table shows the difference between the JPG files converted by the tools compared to JPG files converted by photoshop. 5 Images have been compared.

Info | Photoshop | Sips | Darktable | Raw-therapee | Image Magick | UFraw
------------ | ------------- | ------------ | ------------- | ------------ | ------------ | ------------
Kind | identical |identical |identical | identical|identical | identical
Size | 2.881.969 bytes (EXIF)/ 2.7M (ls -alh)| 2.673.777 bytes / 2.5M |  5.446.710 bytes / 5.2M always bigger | 3.467.998 bytes / 3.3M |  3.467.998 bytes / 3.3M | 1.558.587 bytes / 1.5M always smaller
Where |reference |different |different |different |different| different
Created | reference|different | different| different|different|different
Modified | reference|different | different|different |different|different
Dimension |identical - 5472 × 3648 |5472 × 3648 |different - 5494 × 3666 | different - 5488 × 3662|different - 5496 × 3670|different - 5496 × 3670
Device Make |refrence | identical|identical | identical| not set | not set
Colour space | reference |identical | identical|identical | identical | identical
Colour profile | Adobe RGB (1998)| different - Display P3| different - sRGB |different - RTv2_sRGB|not set | not set
Focal length |reference | identical|identical |identical | not set | not set
Alpha Channel|reference | identical|identical |identical | not set | not set
Red eye |reference | identical|identical |identical | not set | not set
Metering Mode |reference | identical|identical |identical | not set | not set
F number |reference | identical|identical |identical | not set | not set
Exposure program |reference | identical|identical |identical | not set | not set
Exposure time |reference | identical|identical |identical | not set | not set
Latitude |reference | sometimes very small differences|identical |identical | not set | not set
Longitude |reference | sometimes very small differences|identical |identical | not set | not set

## Adobe Photoshop / Lightroom ##

Ideally we can convert images directly with Photoshop or Lightroom, if installed. It seems like Photoshop comes with some sort of command-line automation tool based on NodeJS:
https://github.com/adobe-photoshop/generator-core

See https://photo.stackexchange.com/questions/39532/command-line-approach-to-develop-raw-images-with-adobe-xmp-sidecars

This needs further investigation. Contributions welcome!

## Todo / Reading List ##

- [Compare the quality and XMP compatibility of different RAW converters #65](https://github.com/photoprism/photoprism/issues/65)
- Investigate if PhotoShop or Lightroom can be used to convert images on the command line
- Collect example RAW files of common camera brands and models
- Figure out how we can implement "auto-enhance" (automatically fix exposure and write XMP file while converting to JPEG)
- https://howtogimp.com/raw-photos-in-gimp/ - RAW converters to use with GIMP
- https://github.com/mdouchement/hdr - HDR is a library that handles RAW image format written with Golang
