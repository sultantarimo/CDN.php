# cdn.php

A fast assets manager, that takes a folder and delivers minified & combined assets per type(css/js), works with css &amp; js & .scss(sass). Essentially also a sass compiler.

### Usage

```php
<?php assets('/assets/css/', 'all'); ?>
<?php assets('/assets/js/', 'all'); ?>

// Same as above, (short hand)
<?php assets('/assets/css/'); ?>
<?php assets('/assets/js/'); ?>

// List of files specifically
<?php assets('/assets/css/', 'main.css, header.scss'); ?>
<?php assets('/assets/js/', 'main.js, module.js'); ?>

// Lists files to exclude
<?php assets('/assets/css/', null, 'main.css, header.scss'); ?>
<?php assets('/assets/js/', null, 'main.js, module.js'); ?>

// Specify different folder to save minified folder. default = first param + '/minified/'
<?php assets('/assets/css/', null, null, '/folder/to/save/minfied/'); ?>
<?php assets('/assets/js/', null, null, '/folder/to/save/minfied/'); ?>

// Unminified
<?php assets('/assets/css/', null, null, null, false); ?>
<?php assets('/assets/js/', null, null, null, false); ?>

// Sass output style
<?php assets('/assets/css/', null, null, null, 'nested'); ?>

// Specify main/single source file that imports all your files
<?php assets('/assets/stylesheets/', 'style.scss'); ?>

// Force refresh(normally only updates minified file after changes have occured to source files)
<?php assets('/assets/stylesheets/', null, null, null, null, true); ?>

// Don't echo stylesheet/script links, returns url to minified file
<?php assets('/assets/stylesheets/', null, null, null, null, null, true); ?>

// Using an array
<?php

assets(array(
	'directory' => '',
	/* 
	 * Directory we look for the files specified in 'include' option
	 * Also the default directory for 'output'
	 */

	'include'   => 'all'|string, 
	/* 
	 * Files to include, all/left blank inclues all files in the directory
	 * Can specify specific files i.e main.scss.
	 */

	'exclude'   => null|string,
	/* 
	 * Files to exclude, i.e if you include all files in a folder but want to exclude a few.
	 */

	'output'    => null|string,
	/* 
	 * Where to save the output minified files i.e all.min.css/js
	 * defaults to save in a minified folder created in the directory before 
	 * the specified directory, i.e for '/assets/stylesheets/' it will be /assets/
	 */

	'minify'    => true|bool,
	/* 
	 * true|false, whether to minify the output or not.
	 * can also be a sass output style. can be: "nested", "expanded", "compact", "compressed".
	 */

	'refresh'   => true|bool,
	/* 
	 * true|false, whether to always refresh, only applies to development(doesn't apply on production)
	 * The default behaviour is to refresh only when the source file changes.
	 * Since the default behaviour does not take into account sass import files if you're using one base sass file.
	 * This is a good option to enable of you go that route.
	 */

	'return'    => null|bool
	/* 
	 * Normally a <script>(js) or <link>(css) is echoed to the html
	 * This option disables, the return value of $value = assets(options)
	 * Will be the url to the minified file.
	 */

));
```

### Parameters

1. Location to assets folder i.e for css it could be '/assets/css/' **(required)
2. 'all' or a list of files seperated by (,) comma or left blank(defaults 'all'). **(optional)
3. list of files  to exclude seperated by (,) comma or left blank(defaults to exlcude none) **(optional)
4. location to save '/minified/' assets folder to **(optional)
5. Bool, true || false if you want the assets minified or not, or a sass style "nested | expanded | compact | compressed"

### Output

```html
<script src="/assets/minified/all.min.modified-time-stamp.js">
<link rel="stylesheet" href="/assets/minified/all.min.modified-time-stamp.css">
```

#### Example directory structure

```
.
├─── index.php
├─── assets
│   ├── images
│   ├── css
|   |   |── style.css
│   |   └── libs
|   |		└── library.css
│   └── js
|       |── scripts.js
│       └── libs
 			└── library.js
```



The first parameter i.e '/assets/css/' being the www path to the specific asset folder, the second being a list of files i.e

'scripts.js, main.js' seperated by (,) comma or just 'all' to watch all files changes.

### What does it do?

given '/assets/css/' it looks for all files in the specified directory, minifies them and combines them into one all.min.js/all.min.css
& all.js/all.css.

### What else?

It does Sass, you can write .scss files and they will be evaluated at dev-runtime(it does not run on production) to update all.css.

### Performance?

Apart from the generation of all.js/css files it serves the cached copy if nothing has changed(always defaults to this production), and updates the cached copy only on the first request if something has.


### and?

Files are added to all.js alphabetically, so if you name a file something like ___jquery.js it will come before _second.js or third.js in the minified all.js/css, this can helps with javascript if you want one library that another depends on to come first. Also files ending with .min.ext, i.e *main.min.js* or *main.min.css* do not get processed/minified/sass compiled.
