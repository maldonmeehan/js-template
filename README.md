# EPICODUS STUDENT PROJECT | Modern JS Apps API-Driven App

#### Js-Template, August 4, 2016

#### By _**Maldon Meehan, Epicodus**_

## Description

Template for week one JavaScript. This template includes NPM

#### NPM
This creates a manifest file which is where npm stores a list of packages needed for the project, along with which versions we need. Think of this file as a grocery store list for your project - it keeps track of all the third party packages that it needs to run.

We'll be prompted to enter some information as we create this file. For name we can enter ping-pong. Other than that, we can leave all the defaults for now. You can always edit this file later.

```
$ npm init
```

#### Gulp
Now, let's install our first package, a very important one called gulp. Gulp is a JavaScript package that runs development tasks for us.

Gulp will be in charge of optimizing our code and packaging it up in a format that the browser can understand. While it is an npm package itself, it will also be in charge of using all the other npm packages that we will download later.

Think of gulp as an orchestra conductor. You are the all powerful composer of your program - in our orchestra example you wrote the actual sheet music for the symphony to play. But gulp is the one in charge of telling the horn section when to start, then telling the flute section that it's their turn next, etc.

```
$ npm install gulp --save-dev
```

Add the following to the gulpfile.js file.

| gulpfile.js |
| ------------- |
| var gulp = require('gulp'); |

#### Browserify
Let's download another package with npm! This one is called browserify.

```
$ npm install browserify --save-dev
```

#### Ignoring Packages in Git
So next, we're going to need a .gitignore file. Add the following to the .gitignore file.

| .gitignore |
| ------------- |
| node_modules/ |
| .DS_Store |

#### Vinyl-source-stream
An npm package used for placing browserified source code into a new file.

```
$ npm install vinyl-source-stream --save-dev
```

Add the following to the gulpfile.js file.

| gulpfile.js |
| ------------- |
| var browserify = require('browserify'); |
| var source = require('vinyl-source-stream'); |

Add the following gulp task to the gulpfile.js file.
```
gulp.task('jsBrowserify', ['concatInterface'], function() {
  return browserify({ entries: ['./tmp/allConcat.js'] })
    .bundle()
    .pipe(source('app.js'))
    .pipe(gulp.dest('./build/js'));
});
```

#### Concatenation
A process of copying all JavaScript files into one file to be used in the browser and decrease load time.
```
$ npm install gulp-concat --save-dev
```
| gulpfile.js |
| ------------- |
| var concat = require('gulp-concat'); |

Add the following gulp task to the gulpfile.js file.
```
gulp.task('concatInterface', function() {
  return gulp.src(['./js/*-interface.js'])
    .pipe(concat('allConcat.js'))
    .pipe(gulp.dest('./tmp'));
});
```

#### Minifying with gulp-uglify
The process of removing all unnecessary characters in JS files to optimize JavaScript execution.

```
$ npm install gulp-uglify --save-dev
```
| gulpfile.js |
| ------------- |
| var uglify = require('gulp-uglify'); |

Add the following gulp task to the gulpfile.js file.
```
gulp.task("minifyScripts", ["jsBrowserify"], function(){
  return gulp.src("./build/js/app.js")
    .pipe(uglify())
    .pipe(gulp.dest("./build/js"));
});
```

## Setup/Installation Requirements

* Clone this repository:
* In the command-line run:
```
$ npm install
```
```
$ bower install
```
```
$ gulp build
```
```
$ gulp serve
```
* apiKey: if there is one

## Known Bugs

* Currently no known bugs

## Support and contact details

If you run into any issues or have questions, ideas, or concerns, please feel free to contact Lauren & Maldon on GitHub.

## Technologies Used

* JavaScript
* jQuery
* Bower
* NPM

### License

*MIT License*

Copyright (c) 2016 **_Epicodus_**
