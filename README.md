# dcraw.js
Convert RAW camera images using JavaScript.

This project is a port of [dcraw.c](http://www.cybercom.net/~dcoffin/dcraw/) using [Emscripten](http://emscripten.org).

### Install:
```
npm install dcraw
```

### Usage:

Read an example raw file into a buffer
``` js
const fs = require('fs');
const buf = fs.readFileSync('./example.CR2');
```

##### Get Image Metadata

Parse the file for metadata

``` js
const dcraw = require('dcraw');
dcraw(buf, { verbose: true, identify: true });
```


##### Convert to TIFF

Convert to TIFF file, and save to disk

``` js
const result = dcraw(buf, { exportAsTiff: true });
fs.writeFileSync('example.tiff', result.files['raw_buf.tiff'])
```

### Options

You may use the same options that dcraw supports by providing the flags directly, or you may use the following human friendly versions.

##### Example

``` js
// The following dcraw command, can be written two ways using dcraw.js
// dcraw -T -4 -E <filename>

// Both of these do the same thing
dcraw(buf, { T: true, 4: true, E: true });
dcraw(buf, { exportAsTiff: true, use16BitLinearMode: true, useExportMode: true });
```


|Option|Type|Description|
|------|----|-----------|
|**verbose**|boolean|Print verbose messages|
|**identify**|boolean|Identify files without decoding them (use with '-v' to identify files and show metadata)|
|**toStandardOutput**|boolean|Write image data to standard output|
|**extractThumbnail**|boolean|Extract embedded thumbnail image|
|**updateFileDate**|boolean|Change file dates to camera timestamp|
|**useCameraWhiteBalance**|boolean|Use camera white balance, if possible|
|**useAverageWhiteBalance**|boolean|Average the whole image for white balance|
|**whiteBalanceBox**|x y w h|Average a grey box for white balance|
|**useCustomWhiteBalance**|r g b g|Set custom white balance|
|**useEmbeddedColorMatrix**|boolean|Use/don't use an embedded color matrix|
|**correctChromaticAberration**|r b |Correct chromatic aberration|
|**deadPixelFile**|buffer |Fix the dead pixels listed in this file|
|**darkFrameFile**|buffer |Subtract dark frame (16-bit raw PGM)|
|**setDarknessLevel**|num |Set the darkness level|
|**setSaturationLevel**|num |Set the saturation level|
|**setWaveletThreshold**|num |Set threshold for wavelet denoising|
|**setHighlightMode**|[0-9] |Highlight mode (0=clip, 1=unclip, 2=blend, 3+=rebuild)|
|**setFlipMode**|[0-7] |Flip image (0=none, 3=180, 5=90CCW, 6=90CW)|
|**setColorSpace**|[0-6] |Output colorspace (raw,sRGB,Adobe,Wide,ProPhoto,XYZ,ACES)|
|**setICCFromFile**|buffer |Apply output ICC profile from file|
|**setICCFromCamera**|buffer |Apply camera ICC profile from file or "embed"|
|**useDocumentMode**|boolean|Document mode (no color, no interpolation, no debayer)|
|**useRawMode**|boolean|Document mode without scaling (totally raw)|
|**useExportMode**|boolean|Document mode without cropping|
|**setNoStretchMode**|boolean|Don't stretch or rotate raw pixels|
|**setNoAutoBrightnessMode**|boolean|Don't automatically brighten the image|
|**setBrightnessLevel**|num |Adjust brightness (default = 1.0)|
|**setCustomGammaCurve**|p ts |Set custom gamma curve (default = 2.222 4.5)|
|**setInterpolationQuality**|[0-3] |Set the interpolation quality|
|**setHalfSizeMode**|boolean|Half-size color image (twice as fast as "-q 0")|
|**setFourColorMode**|boolean|Interpolate RGGB as four colors|
|**setMedianFilter**|num |Apply a 3x3 median filter to R-G and B-G|
|**setImageCount**|[0..N-1] |Select one raw image or "all" from each file|
|**use16BitMode**|boolean|Write 16-bit instead of 8-bit|
|**use16BitLinearMode**|boolean|Linear 16-bit, same as "-6 -W -g 1 1"|
|**exportAsTiff**|boolean|Write TIFF instead of PPM|
