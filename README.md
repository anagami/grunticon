# Grunticon
[![Build Status](https://travis-ci.org/filamentgroup/grunticon.png?branch=master)](https://travis-ci.org/filamentgroup/grunticon)

```
         /'
        //
    .  //
    |\//7
   /' " \
  .   . .
  | (    \
  |  '._  '
  /    \'-'

```

### A mystical CSS icon solution

grunticon is a [Grunt.js](https://github.com/cowboy/grunt/) task that makes it easy to manage icons and background images for all devices, preferring HD (retina) SVG icons but also provides fallback support for standard definition browse

grunticon takes a [folder of SVG/PNG files](https://github.com/filamentgroup/grunticon/tree/master/example/source) (typically, icons that you've drawn in an application like Adobe Illustrator), and [outputs them](https://github.com/filam

grunticon also generates [a small bit of JavaScript and CSS](https://github.com/filamentgroup/grunticon/blob/master/example/output/grunticon.loader.txt) to drop into your site, which asynchronously loads the appropriate icon CSS dependin

You can see [a demonstration of the output here](http://filamentgroup.github.com/grunticon/example/output/preview.html).

## Before you get started!

[Have you seen Grumpicon?](http://grumpicon.com)

[<img src="http://filamentgroup.com/images/grunticon_workflow_grumpicon.jpg" width="400">](http://grumpicon.com)

[Grumpicon](http://grumpicon.com) is a browser-based app that performs much of the functionality of Grunticon through a simple drag and drop interface. It's much easier to set up than Grunticon, and sometimes, it's all you need (though i

If you're interested in trying out Grumpicon, you might be interested in this handy guide as well: [Grumpicon Workflow](http://filamentgroup.com/lab/grumpicon_workflow/)

## Getting Started
This plugin requires Grunt `~0.4.2`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-grunticon --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-grunticon');
```

## The "grunticon" task

### Required configuration properties

grunticon has a files object that needs to be filled in order to run,
this files object currently requires that a cwd, and a dest dir are
placed, and therefore will blow up without it. This will be fixed to
better fit the pattern set by Grunt for this.

These can be set in your Gruntfile.js config file. Grunticon is a multitask, making it easy to create as many export batches as you'd like. Just create one or more custom named objects within the `grunticon` object, and configure Grunticon `options` within it, like so:

```
	grunticon: {
    myIcons: {
			files: [{
				expand: true,
				cwd: 'example/source',
				src: ['*.svg', '*.png'],
				dest: "example/output"
			}],
  		options: {
      }
    }
	}
```

**IMPORTANT NOTE:** grunticon will overwrite any files in the `dest` directory if they are of the same name as a file that grunticon needs to create. For easiest results, you can set `dest` to a folder that does not yet exist in your directory and grunticon will create that folder, or set it to an existing folder and be sure to configure grunticon to create file names that do not already exist in that folder.

With these configuration properties set, you can add `grunticon` to your default tasks list. That'll look something like this:

    grunt.registerTask('default', 'lint qunit concat min
grunticon:myIcons');

grunticon will now batch your icons whenever you run grunt.

### Options

#### options.datasvgcss
Type: `String`
Default value: `"icons.data.svg.css"`

The name of the generated CSS file containing SVG data uris.

#### options.datapngcss
Type: `String`
Default value: `"icons.data.png.css"`

The name of the generated CSS file containing PNG data uris

#### options.urlpngcss
Type: `String`
Default value: `"icons.fallback.css"`

The name of the generated CSS file containing external png url references.

#### options.previewhtml
Type: `String`
Default value: `"preview.html"`

The name of the generated HTML file containing PNG data uris.

#### options.loadersnippet
Type: `String`
Default value: `"grunticon.loader.txt"`

 The name of the generated text file containing the grunticon loading snippet.

#### options.pngfolder
Type: `String`
Default value: `"png/"`

 The name of the generated folder containing the generated PNG images.

#### options.cssprefix
Type: `String`
Default value: `"icon-"`

a string to prefix all css classes with.

#### options.customselectors
Type: `Object`

Allows you to specify custom selectors (in addition to the generated `cssprefix + filename - extension` class) for individual files. 

Example:

```
{
	"foo": [".icon-bar", ".baz"]
}
```

will produce:

```
.icon-bar,
.bar,
.icon-foo{
	//css
}
```

#### options.defaultWidth
Type: `String`
Default value: `"400px"`

a string that MUST be defined in px that will be the size of the PNG if there is no width given in the SVG element.

#### options.defaultHeight
Type: `String`
Default value: `"300px"`

similar to defaultWidth, but for height

#### options.colors

Allows you to predefine colors as variables that can be used in filename color configuration.


#### Automating color variations

Grunticon allows you to output any icon in different colors simply by changing its filename to the following syntax: `myfilename.colors-red-aa0000-gray.svg`. In this example, any color names or hexidecimal values that follow `colors-` and are separated by a dash will be used to generate additional icons of that color. By default, each icon will be assigned a numbered class name for CSS use. You can improve the class naming conventions by defining color variables in your gruntfile's `colors` option shown above. When defined, you can reference a color variable in place of a color in your file names, and the generated classes will use that variable name as well. See the `gruntfile.js`'s `colors` option and the sample bear svg for an example of color automation.

*A note on filesize impact:* Adding color variations of an icon involves creating duplicates of that icon's SVG source in the CSS, so unfortunately, each color variation will cause an increase in filesize. However, transferring CSS with gzip compression can negate much of this filesize increase, and we highly recommend always transferring with gzip. In testing, we found that creating a color variation of every icon in our example set increased overall size by 25%, rather than 100% as a raw text duplicate would increase. That said, size increases for non-SVG-supporting browsers will be more dramatic, as the fallback PNGs will not have the heavy transfer compression as SVG enjoys. We advise using this feature on a case-by-case basis to ensure overhead is kept to a minimum.

## Browser testing results for icon output

The generated asynchronous CSS loader script delivers an appropriate icon stylesheet depending on a device/browser's capabilities. Grunticon is supported in cases where icon fonts fail.

Browsers that render the SVG data url stylesheet:
- IE9
- Chrome 14+ (maybe older too?)
- Safari 4+ (maybe older too?)
- Firefox 3.6+ (maybe older too?)
- Opera 15+ 
- iOS 3+ Safari and Chrome
- Android 4.0 Chrome (caveat: SVG icons do not scale in vector, but do appear to draw in high-resolution)
- Android 4.0 ICS Browser
- BlackBerry Playbook

Browsers that receive the PNG data url stylesheet:
- IE8
- All versions of Opera, Opera Mini, and Opera Mobile before Chrome integration (v 15)
- Android 2.3 Browser
- Android 2.2 Browser
- Android 2.1 Browser
- Android 1.6 Browser
- Android 1.5 Browser

Browsers that receive the fallback png request:
- IE7
- IE6
- Non-JavaScript environments

View the full support spreadsheet [here](https://docs.google.com/spreadsheet/ccc?key=0Ag5_yGvxpINRdHFYeUJPNnZMWUZKR2ItMEpRTXZPdUE#gid=0). Feel free to edit it if you find anything new.

The test page can be found [here](http://filamentgroup.com/examples/grunticon-icon-test/).

## Tips

### Serving compressed CSS
One of the great benefits to data uris is the ability to compress the images heavily via gzip compression. Be sure to enable gzip of CSS on your server, as it'll cut your icon transfer size greatly.

### Creating SVG Artwork

The workflow we've been using so far involves creating a new Illustrator file with the artboard set to the desired size of the icon you want set in the CSS. 

Export the artwork by choosing File > Save as...  In the dialog, choose "SVG" as the format and enter a name for the file (this wil be used as your class name later, so keep it free of any disallowed CSS class characters like `.`, `{`, `(`, `)`, etc.

In the Save SVG dialog that opens up, there are lots of options. SVG has a ton of formats, so here are a few tips we've learned. 

- SVG Profile: Seems like SVG 1.1 Tiny is really well supported across even older mobile platforms so if you have simple artwork that doesn't use gradients or opacity this will yield a smaller and more compatible graphic. If you want to use all the fancy effects, save artwork as SVG 1.1. 
- Type: Convert to outline before export.
- Subsetting: None, I usually convert all text to outlines ahead of time
- Images: Embed
- Don't check "Preserve Illustrator editing" to reduce file size

## Warnings
* If your files have `#`, `.`, `>`, or any other css selecting character in their names, they will likely be improperly processed.

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

## Release History
* Version 1.0.0: Almost complete rewrite. Breaking out pieces of this
project into other areas
* Version 0.6.5: CSS Writing has been moved from Phantom to Node, in order to decrease base64 datauri sizes
* Version 0.6.0: Grunticon now comes with PNG Crush. This will reduce the size of your SVGs
* Version 0.5.0: Grunticon now comes with SVGO. This cleans up your SVGs, greatly reducing the size of your CSS file.
* Version 0.4.1: Opera browsers prior to version 15 are given fallback PNG due to SVG scaling troubles.
* Version 0.4.0: Automated filename-driven color variations were added, along with the `colors` option
* Version 0.3.4: SVGs without width and height can be used
* Version 0.3.2: Added PhantomJS as a Node dependency, easing installation
* Version 0.3.1: Documentation updates
* Version 0.3.0: Grunticon becomes a multitask - syntax change involved in gruntfile
* Version 0.2.1: Custom selectors feature added
* Version 0.2.0: Compatibility rewrite for Grunt 0.4x
* Version 0.1.6: Switched from base64 encoding to escaping raw SVG text in data uris. Fixes to cssprefix setting. If fallback png data uri is > 32768 chars, link to ext png instead for IE issues.
