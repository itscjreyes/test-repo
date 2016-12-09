#SalesHub - Development Styleguide

- [Local Development](#local-development)
  - [Local File Structure for Gulp](#local-file-structure-for-gulp)
  - [Gulp](#gulp)
    - [Installing Gulp](#installing-gulp)
    - [Gulp Plugins](#gulp-plugins)
    - [Starting Gulp](#starting-gulp)
  - [FTP](#ftp)

##Local Development

###Local File Structure

*__Note:__* Maintaining this file structure is important for the Gulp code below to work.

    |--local-hubl-server
       |--bin
       |--conf
       |--docs
       |--lib
       |--work
           |--hubthemes
             |--vast
                |--custom
                   |--blog
                   |--dev
                      |--base
                      | *Include base HubSpot, Foundation, etc SCSS partials*
                      |--global
                      | *Include global SCSS partials, such as "__header.scss", "__mediaQueries.scss"*
                      |--pages
                      | *Include partials for each page*
                         |--[main.scss]
                         | *This includes main variables and styles*
                         |--[main.js]
                         | *This is the working JS file. Note that $(window).load() may need to replace $(document).ready() during development in order for functions to work. This should be changed back in the final file.*
                   |--email
                   |--page
                      |--landing-page-basic
                      | *Include HTML files for landing page templates*
                      |--web-page-basic
                      | *Include HTML files for web page templates*
                   |--styles
                      |--default
                         |--[hs__default__custom__style.css]
                         |--[main.js]
                         | *These are the final CSS/JS files to upload*
                   |--system
                      |--error-pages
                      |--global
                      | *This includes global modules such as "header.html"*
                      |--password-pages
                      |--subscription-preferences

The main files you will work on will be any HTML pages located in the __blog__, __email__, or __page__ folders, as well as the Sass and JS files located in the __dev__ folder.

###Gulp

####Installing Gulp

By developing locally, we are able to automate our workflow with Gulp.

To install Gulp on your system (this will only need to be done once per computer), first check to see if Node is installed and what version you have. Run the following command in the command line:

    node -v

To install Node, download it from [Node.js](https://nodejs.org/en/).

Next, install Gulp on your system by running this command:

    npm install gulp -g

To initialize Gulp in a project (this will need to be done once for each project), go to the theme (__vast__) folder and type the following command:

    npm init

This will create a __package.json__ file.

Next, create a Gulp file:

    touch gulpfile.js

Run the following command to install Gulp in the project folder:

    npm install gulp --save-dev

####Gulp Plugins

There are several different plugins we can use to help automate our workflow. We will be using a Sass compiler, autoprefixer, ES6 to ES5 transpiler, and a live reload tool called Browsersync.

Install these plugins by running the following commands:

*Sass compiler*

    npm install gulp-sass gulp-concat --save-dev

*Autoprefixer*

    npm install gulp-autoprefixer --save-dev

*Babel*

    npm install gulp-babel babel-preset-es2015 --save-dev

*Browsersync*

    npm install browser-sync --save-dev

In your __gulpfile.js__ file, enter the following code:

```javascript
var gulp = require('gulp');
var sass = require('gulp-sass');
var concat = require('gulp-concat');
var babel = require('gulp-babel');
var autoprefixer = require('gulp-autoprefixer');
var browserSync = require('browser-sync').create();
var reload = browserSync.reload;

gulp.task('styles', function () {
  return gulp.src('./custom/dev/**/*.scss')
    .pipe(sass().on('error', sass.logError))
    .pipe(autoprefixer('last 2 version', 'safari 5', 'ie 8', 'ie 9', 'opera 12.1'))
    .pipe(concat('hs_default_custom_style.css'))
    .pipe(gulp.dest('./custom/styles/default/'))
    .pipe(reload({stream: true}));
});

gulp.task('scripts', function() {
  return gulp.src('./custom/dev/*.js')
    .pipe(babel({
      presets: ['es2015']
    }))
    .pipe(gulp.dest('./custom/styles/default/'))
    .pipe(reload({stream: true}));
});

gulp.task('watch', function() {
  gulp.watch('./custom/dev/**/*.scss', ['styles']);
  gulp.watch('./custom/dev/*.js', ['scripts']);
  gulp.watch('./custom/**/**/*.html', reload);
});

gulp.task('browser-sync', function() {
  browserSync.init({
    proxy: '127.0.0.1:8080'
  })
});

gulp.task('default', ['browser-sync','styles','scripts','watch']);
```

####Starting Gulp

To run Gulp, first start the Local Hubl Server in the command line:

    [your path]/local-hubl-server/bin/local-hubl-server

This will open http://localhost:8080 in your browser. However, because we want to use Gulp, we need to run it at the same time.

In a new terminal window, navigate to the theme (__vast__) folder, then run the following command:

    gulp

This will open http://localhost:3000 in your browser. You are now free to develop locally! Gulp will compile your Sass files, check for any errors, and automatically reload the page when you save any changes to your HTML, SCSS, and JS files.

###FTP

1. Connect to FTP:
  - __Host__: ftp.hubapi.com
  - __Port__: 3200
  - __Protocol__: FTP
  - __Encryption__: Require explicit FTP over TLS
  - __Logon Type__: Normal
  - __User__: [hubspot user id]
  - __Password__: [hubspot password]

2. Navigate to the correct __portal__ > __content__ > __templates__ > __custom__

3. Global modules will need to be uploaded first.
  - From the __custom__ folder, navigate to __system__ > __global__
  - Transfer all files in the __global__ folder from the local site to the remote site

4. Page transfers
  - Transfer all HTML files from the __blog__, __email__, __page__ folders to the corresponding folders in the remote site

5. CSS and JS transfers
  - The final/compiled CSS and JS files are located in __custom__ > __styles__ > __default__
  - Transfer these files to the corresponding folder in the remote site

6. The transferred files will be located in Coded Files in the HubSpot Design Manager
  - The CSS file is already named to be the primary stylesheet, so it should automatically link
  - The JS file will need to be linked in the footer