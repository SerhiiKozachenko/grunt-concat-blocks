grunt-concat-blocks
============

## Description

Use build blocks to do concatenation in html, jade, razor or any other files:

Utilize build blocks in your html to indicate the files to be concatenated. This task will parse the build
blocks in your html, and it will schedule the concatenation of the desired files by dynamically updating the `concat` task and then run it.

**This task DOESN'T modifies files, it only perform files concatenation in build blocks.**

## Usage

Example usage with grunt.init:

**in `Gruntfile.js`:**

```javascript
    concatBlocks: {
        // specify which files contain the build blocks
        html: 'views/layout.jave',
        // explicitly specify the parent directory you are working in
        // this is the the base of your links ( "/" )
        root: 'public'
    }
```

Below are example corresponding build blocks in an example referenced html file. Multiple build blocks may be
used in a single file. The grunt templating engine can be used in the build file descriptions. The data passed to the
template processing is the entire config object.

**in an html file within the `output` directory**

```html
<!-- build:css /css/combined.css -->
<link href="/css/one.css" rel="stylesheet">
<link href="/css/two.css" rel="stylesheet">
<!-- endbuild -->

<!-- build:js scripts/combined.<%= grunt.file.readJSON('package.json').version %>.concat.min.js -->
<!-- You can put comments in here too -->
<script type="text/javascript" src="scripts/this.js"></script>
<script type="text/javascript" src="scripts/that.js"></script>
<!-- endbuild -->

<!-- build:js scripts/script1.<%= grunt.template.today('yyyy-mm-dd') %>.min.js -->
<script type="text/javascript" src="scripts/script1.js"></script>
<!-- endbuild -->
```

The example above has three build blocks on the same page. The first two blocks concat and minify. The third block
minifies one file. They all put the new scripts into a newly named file. The original JavaScript files remain untouched
in this case due to the naming of the output files.

You can put comments and empty lines within build blocks.

Assuming your `package.json.version` is `0.1.0` after the bump, and it is October 31, 2012 running `grunt grunt-concat-blocks` would
create the following three files:

```bash
# concat and minified one.css + two.css
output/css/combined.css

# concat and minified this.js + that.js
output/scripts/combined.0.1.0.concat.min.js

# minified script1.js
output/scripts/script1.2012-10-31.min.js
```

Also the html in the file with the build blocks would be updated to:

```html
<link href="/css/combined.css" rel="stylesheet">

<script type="text/javascript" src="scripts/combined.0.1.0.concat.min.js"></script>

<script type="text/javascript" src="scripts/script1.2012-10-31.min.js"></script>
```


## Installation and Use

To use this package run `npm install grunt-concat-blocks --save-dev`.

Then load the grunt task in your `grunt.js`

```javascript
grunt.loadNpmTasks('grunt-concat-blocks');
```

If you use `grunt-concat-blocks` you can ommit using `loadNpmTask` for the 'grunt-contrib-concat' plugin:

`grunt-concat-blocks` will load the above plugin for you.

---

[NPM module](https://npmjs.org/package/grunt-concat-blocks)
