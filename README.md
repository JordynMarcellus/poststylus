# PostStylus
[![NPM version][npm-image]][npm-url] [![Build Status][travis-image]][travis-url] [![Dependency Status][daviddm-image]][daviddm-url]

PostStylus is a [PostCSS](https://github.com/postcss/postcss) adapter for Stylus. With it you can use any PostCSS plugin as a transparent Stylus plugin. Neato!

It loads PostCSS plugins into Stylus just before it compiles output css into a file. If you use sourcemaps, they are preserved and extended by PostCSS processing as well.

Inspired by [autoprefixer-stylus](https://github.com/jenius/autoprefixer-stylus)

--

### Install
```sh
$ npm install --save poststylus
```

--

### Usage
Just use `poststylus` as a regular stylus plugin and pass it an array of postcss plugins, like this:
```js
stylus(css).use(poststylus([
  // your postcss plugins here
]))
```

###### Gulp:
```js
var gulp = require('gulp'),
    stylus = require('gulp-stylus'),
    poststylus = require('poststylus');

// PostCSS plugins we want to apply
var postcssPlugins = [
    require('autoprefixer')(),
    require('postcss-position')(),
    require('lost')()
];

gulp.task('stylus', function () {
  gulp.src('style.styl')
    .pipe(stylus({
      use: [
        poststylus(postcssPlugins)
      ]
    }))
    .pipe(gulp.dest('.'))
});

gulp.task('default', ['stylus']);
```

  
###### Grunt:
``` js
module.exports = function(grunt) {
  
  // PostCSS plugins we want to apply
  var postcssPlugins = [
    require('autoprefixer')(),
    require('postcss-position')(),
    require('lost')()
  ];

  grunt.initConfig({

    stylus: {
      compile: {
        options: {
          use: [
             poststylus(postcssPlugins)
          ]
        },
        files: {
          'style.css': 'style.styl'
        }
      }
    }

  });

  grunt.registerTask('default', ['stylus']);

};
```
--
### Alternate syntax
Alternatively, you can pass plugins in by their module name, e.g. 
```js
poststylus([
	'postcss-position',
	'postcss-hexrgba'
]);
```
the modules will then be automatically required and executed e.g. `'postcss-position'` will become `require('postcss-position')()`
-- 

### Custom PostCSS
You can do any custom javascript/PostCSS processing of stylus output you want with PostStylus, just declare an on-the-fly plugin like so:
```js
var myPostcss = postcss.plugin('custom', function() {
  return function (css) {
    // custom processing here
  });
};

// then pipe it into poststylus, as above
stylus(css).use(poststylus(
  require(myPostcss())
)
```
Refer to the [PostCSS Docs][postcss-link] for more.

-- 

### License

MIT © [Sean King](http://simpla.io)


[npm-image]: https://badge.fury.io/js/poststylus.svg
[npm-url]: https://npmjs.org/package/poststylus
[travis-image]: https://travis-ci.org/seaneking/poststylus.svg?branch=master
[travis-url]: https://travis-ci.org/seaneking/poststylus
[daviddm-image]: https://david-dm.org/seaneking/poststylus.svg?theme=shields.io
[daviddm-url]: https://david-dm.org/seaneking/poststylus
[postcss-link]: https://github.com/postcss/postcss
