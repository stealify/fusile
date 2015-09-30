# fusile

[![NPM Version][npm-image]][npm-url]
[![NPM Downloads][downloads-image]][downloads-url]
[![Linux Build][travis-image]][travis-url]
<!--[![Windows Build][appveyor-image]][appveyor-url]-->
[![Test Coverage][coveralls-image]][coveralls-url]
[![Join the chat][gitter-image]][gitter-url]

A web asset precompiling file system proxy.

Mounts a fuse file system on a target directory, auto loads available compilers, compiles assets on request, caches compiled assets if source file is unmodified, watches original file for changes on demand.

Turn your setup of custom configured transpiler plugins for all your different tools from this:

```
+---------------+
| Source.es6.js |  -+--> Transpiler --> Module loader --> Browser
+---------------+   |
                    +--> Watcher --> Transpiler --> Livereload
                    |
                    +--> Transpiler --> Linter
                    |
                    +--> Transpiler --> Test runner
                    |
                    +--> Transpiler --> Build system
```

into this:

```
+---------------+      +--------------------------+
| Source.es6.js |  --> | Source.transpiled.es6.js | -+--> Module loader --> Browser
+---------------+      +--------------------------+  |
                                                     +--> Watcher --> Livereload
                                                     |
                                                     +--> Linter
                                                     |
                                                     +--> Test runner
                                                     |
                                                     +--> Build system
```

## Installation

Prerequisites: [Fuse](https://github.com/bcle/fuse4js#requirements)

```
npm install -g fusile
```

## Usage

**General usage:** `fusile <sourceDir> <mountPoint>`

Fusile is a compiling file system proxy. In order to get it to compile your assets you need to install one or more of these precompilers: `LiveScript`, `babel`, `coco`, `coffee-script`, `dogescript`, `less`, `marked`, `myth`, `node-sass`, `stylus`, `swig`

When any of the above precompilers are available in the context of fusile (in your project root or globally installed), fusile will automatically load it and start compiling files with the corresponding file extension when you read the files from the mountpoint.

Once you've mounted the new transpiled source directory, point your browser, web server, linter, test runner directly at the files in it, and you are ready to go.


## Project status

Status: Fairly well tested. Need real world exposure. Feedback appreciated!

## Roadmap

**[v1.x](https://github.com/Munter/fusile/issues?q=is%3Aopen+is%3Aissue+milestone%3Av1.x)**

 - [x] Autoload installed transpilers
 - [x] Asset compiling on demand
 - [x] Intelligent caching of compiled assets
 - [x] File watcher based cache busting (optional)
 - [x] Autoprefixer
 - [x] Sourcemaps
 - [x] Compiled file extension rewriting to target extension
 - [x] Isolate Accord transpiler autoloading into separate module

**[v2.x](https://github.com/Munter/fusile/issues?q=is%3Aopen+is%3Aissue+milestone%3Av2.x)**
 - [ ] Expose a compile lifecycle callback to let people hook in their own pipeline
 - [ ] Configuration: Options for individual transpiler
 - [ ] Configuration: File extension / glob pattern to transpiler mapping
 - [ ] Detect compiled dir file watcher init events and proxy them through to source dir

**[v3.x](https://github.com/Munter/fusile/issues?q=is%3Aopen+is%3Aissue+milestone%3Av3.x)**
 - [ ] Refactoring and documentation
 - [ ] Stability research and improvements
 - [ ] Performance research and improvements


## File watching

**General usage:** `fusile <sourceDir> <mountPoint> --watch "**/*.jsx" --watch "**/*.less" -w "**/*.stylus" -w "**/*.scss"`

You can add file watches to files in the Fusile mountpoint. Because Fusile can't yet pick up your file watchin events you will need to specify glob patterns like above, which will set up file watchers on the matching patterns.

File watchers will be set up per individual file, and only at the time when you open a file for reading. This should ensure a miniumum amount of inode watches, and a minimum amount of recompiles.

Note that watcher glob patterns are quoted to avoid shell expansion of the glob patterns at execution time.

## License
MIT


[npm-image]: https://img.shields.io/npm/v/fusile.svg
[npm-url]: https://npmjs.org/package/fusile
[downloads-image]: https://img.shields.io/npm/dm/fusile.svg
[downloads-url]: https://npmjs.org/package/fusile
[travis-image]: https://img.shields.io/travis/Munter/fusile/master.svg?label=linux
[travis-url]: https://travis-ci.org/Munter/fusile
[appveyor-image]: https://img.shields.io/appveyor/ci/Munter/fusile/master.svg?label=windows
[appveyor-url]: https://ci.appveyor.com/project/Munter/fusile
[coveralls-image]: https://img.shields.io/coveralls/Munter/fusile/master.svg
[coveralls-url]: https://coveralls.io/r/Munter/fusile?branch=master
[gitter-image]: https://badges.gitter.im/Join%20Chat.svg
[gitter-url]: https://gitter.im/Munter/fusile?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge
