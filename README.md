# mcr-dl

Command-line tile downloader/assembler for [MyCoRe](https://github.com/MyCoRe-Org/mycore) tiled images.

Download full-resolution images for a given MyCoRe `.xml` URL.

MyCoRe provides a [tool for slicing images into tiles](https://github.com/MyCoRe-Org/image-tiler/blob/master/src/main/java/org/mycore/imagetiler/MCRImage.java). This re-assembles those tiles as served up by MCRTileServlet into a full image.

See also: [dzi-dl](https://github.com/ryanfb/dzi-dl), [iiif-dl](https://github.com/ryanfb/iiif-dl), [dezoomify](https://github.com/lovasoa/dezoomify), [dezoomify-rs](https://github.com/lovasoa/dezoomify-rs)

## Requirements

 * Ruby
 * [Bundler](http://bundler.io/)
 * [ImageMagick](http://www.imagemagick.org/)
 
## Usage

    bundle exec ./mcr-dl.rb 'http://example.com/servlets/MCRTileServlet/example_derivate_00001234/example_filename_300.jpg/imageinfo.xml'

To find a `.xml` URL for a given Deep Zoom image viewer, you may need to open your web browser's Developer Tools and go to e.g. the "Network" pane, then reload the page and see what resources are loaded via AJAX.

Alternately, if you have [PhantomJS](http://phantomjs.org/) installed, you can use `xmlreqs.js` to list all URLs ending in `.xml` requested by a given webpage URL:

    phantomjs xmlreqs.js 'http://example.com/viewer/example_derivate_00001234/example_filename_300.jpg'

## Docker Usage

There's also [an automated build for this repository on Docker Hub at `ryanfb/mcr-dl`](http://hub.docker.com/r/ryanfb/mcr-dl). It defines an `ENTRYPOINT` which will start `mcr-dl.rb` and pass any other arguments or environment variables to it, as well as defining a `/data` volume which you can map to your host to store manifests and images. For example, to download an image into the current directory:

    docker run -t -v $(pwd):/data ryanfb/mcr-dl 'http://example.com/servlets/MCRTileServlet/example_derivate_00001234/example_filename_300.jpg/imageinfo.xml'
