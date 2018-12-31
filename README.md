# [<img title="skipper-better-s3 - Amazon S3 filesystem adapter for Skipper" src="http://i.imgur.com/P6gptnI.png" width="200px" alt="skipper emblem - face of a ship's captain"/>][project-root] Skipper-S3-GM

# skipper-s3-gm

Based on SailsJS Skipper S3 adapter for receiving [upstreams](https://github.com/balderdashy/skipper#what-are-upstreams) and orginally forked from [skipper-s3-resize](https://github.com/basicinception/skipper-s3-resize) but with MANY EXTRA features.

* Added GraphicsMagick CROP, RESIZE, noProfile(Remove EXIF data) option
* Added file rename option before it is uploaded to S3
* S3 upload addition information (region, endpoint, token, maxBytes, headers)
* Updated all depeneded packages to the latest version

# Requirements
## Getting started
First download and install [GraphicsMagick](http://www.graphicsmagick.org/) or [ImageMagick](http://www.imagemagick.org/). In Mac OS X, you can simply use [Homebrew](http://mxcl.github.io/homebrew/) and do:

## Debian/Ubuntu
```
apt-get install graphicsmagick
```
## MacOS X
```
brew install graphicsmagick
```

## Installation

First of all, make sure you have graphicmagick installed.

To install it using npm:
```
$ npm install skipper-s3-gm --save
```

Also make sure you have skipper itself [installed as your body parser](http://beta.sailsjs.org/#/documentation/concepts/Middleware?q=adding-or-overriding-http-middleware).

## Usage
1. In Controller:
```javascript
  upload: function(req, res) {
    req.file('image').upload({
      adapter: require('skipper-s3-gm'),
      key: <YOUR_S3_KEY>,
      secret: <YOUR_S3_SECRET>,
      bucket: <YOUR_S3_BUCKET>,
      region: <YOUR_S3_REGION_OPTIONAL>,
      endpoint: <YOUR_S3_ENDPOINT_OPTIONAL>,
      token: <YOUR_S3_TOKEN_OPTIONAL>,
      maxBytes: 1000 * 1000 * 10,   // [OPTIONAL]: S3 max upload size
      headers: {                    // [OPTIONAL]: S3 Additional Headers information
        'customkey': 'data'         
      },
      gm: {                         // [OPTIONAL]: You can skip all graphicsmagick options
        filename: function(file) {  // [OPTIONAL]: input can be wither FUNCTION or STRING
           return file.filename + '_awesome';
        },
        format: 'jpg',              // [OPTIONAL]: if you need to change the image format 
        noProfile: true,            // [OPTIONAL]: remove EXIF profile data (Default = false)
        crop: {                     // [OPTIONAL] Crop
          width: 200,
          height: 200,
          x: 0,
          y: 0
        },
         resize: {                  // [OPTIONAL] Resize
          width: 200,
          height: 200,
          options: '!'              // DOC: http://aheckmann.github.io/gm/docs.html#resize
        }
      }
    }, function(err, uploadedFiles) {
      if(err) return res.serverError(err);
        res.ok();
      });
    }
```

## Options
1. Refer [skipper documentations.](https://github.com/balderdashy/skipper#uploading-files-to-s3)