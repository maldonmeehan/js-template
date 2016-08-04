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

#### Environmental Variables | gulp-util
A variable that indicates which environment - production or development, for instance - is being referenced.
gulp-util: A npm package that manages multiple utilities, including environmental variables.


```
$ npm install gulp-util --save-dev
```
| gulpfile.js |
| ------------- |
| var utilities = require('gulp-util'); |
| var buildProduction = utilities.env.production; |

Build Tasks

```
gulp.task("build", ['clean'], function(){
  if (buildProduction) {
    gulp.start('minifyScripts');
  } else {
    gulp.start('jsBrowserify');
  }
});
```

Clean Tasks
```
$ npm install del --save-dev
```
| gulpfile.js |
| ------------- |
| var del = require('del'); |

```
gulp.task("clean", function(){
  return del(['build', 'tmp']);
});
```

#### Linting with JSHint
Linter: A tool that analyzes your code and warns you about parts that don't follow stylistic conventions or could cause bugs in the future.

JSHint: A specific linter tool that can be installed with an npm package.

```
$ npm install jshint --save-dev
$ npm install gulp-jshint --save-dev
```

gulpfile.js
```
var jshint = require('gulp-jshint');

gulp.task('jshint', function(){
  return gulp.src(['js/*.js'])
    .pipe(jshint())
    .pipe(jshint.reporter('default'));
});
```

#### Bower
A package manager (much like npm) specifically meant for front-end packages like Bootstrap and jQuery.
```
$ npm install bower -g
$ bower init
```

#### jQuery
```
$ bower install jquery --save
$ bower install
```

.gitignore
```
bower_components
```

#### Bootstrap
```
$ bower install bootstrap --save
```
index.html should look like this:
```
<script src="bower_components/jquery/dist/jquery.min.js"></script>
<link rel="stylesheet" href="bower_components/bootstrap/dist/css/bootstrap.min.css">
<script src="bower_components/bootstrap/dist/js/bootstrap.min.js"></script>
<script type="text/javascript" src="build/js/app.js"></script>
```

#### Moment.js
```
$ bower install moment --save
```
add to index.html
```
<script src="bower_components/moment/min/moment.min.js"></script>
```
add to time-interface.js
```
$(document).ready(function(){
  $('#time').text(moment());
});
```
add to index.html
```
<h3>Well look at the time! </h3>
<h3 id="time"></h3>
```

#### Bower Packages
A package manager (much like npm) specifically meant for front-end packages like Bootstrap and jQuery.
```
$ npm install bower-files --save-dev
```

```
var lib = require('bower-files')({
  "overrides":{
    "bootstrap" : {
      "main": [
        "less/bootstrap.less",
        "dist/css/bootstrap.css",
        "dist/js/bootstrap.js"
      ]
    }
  }
});

gulp.task('build', ['clean'], function(){
  if (buildProduction) {
    gulp.start('minifyScripts');
  } else {
    gulp.start('jsBrowserify');
  }
  gulp.start('bower');
});

gulp.task('bowerJS', function () {
  return gulp.src(lib.ext('js').files)
    .pipe(concat('vendor.min.js'))
    .pipe(uglify())
    .pipe(gulp.dest('./build/js'));
});


gulp.task('bower', ['bowerJS', 'bowerCSS']);
```

####
BowerSync
An npm package used to implement development servers with live reloading capabilities.
```
$ npm install browser-sync --save-dev
```
```
var browserSync = require('browser-sync').create();

gulp.task('serve', function() {
  browserSync.init({
    server: {
      baseDir: "./",
      index: "index.html"
    }
  });
});

```

#### Sass
```
$ npm install gulp-sass gulp-sourcemaps --save-dev
```
```
var sass = require('gulp-sass');
var sourcemaps = require('gulp-sourcemaps');

gulp.task('cssBuild', function() {
  return gulp.src(['scss/*.scss'])
    .pipe(sourcemaps.init())
    .pipe(sass())
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('./build/css'));
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
