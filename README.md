meta-iojs
=========

OpenEmbedded layer for latest stable [io.js](https://iojs.org/ "io.js")  releases.

[![Flattr this git repo](http://api.flattr.com/button/flattr-badge-large.png)](https://flattr.com/submit/auto?user_id=imyller&url=https://github.com/imyller/meta-iojs&title=meta-iojs&language=&tags=github&category=software)

## io.js versions

Latest stable release of:
* `1.2`
* `1.3`
* `1.4`
* `1.5`
* `1.6`

## Packages

 * `iojs`
 * `iojs-npm`
 * `iojs-dtrace`
 * `iojs-systemtap`

Installation
============

1. Add `meta-iojs` layer to `sources/layers.txt`

	```
	  meta-iojs,git://github.com/imyller/meta-iojs.git,master,HEAD
	```
		
2. Add `meta-iojs` layer to `EXTRALAYERS` in `conf/bblayers.conf`

	```
		EXTRALAYERS +=" \
			${TOPDIR}/sources/meta-iojs \
		"
	```
  
3. Run `oebb.sh update`

Usage
=====

### Building io.js package

1. To build latest stable io.js package:

```
	bitbake iojs
```

### io.js runtime as a dependency

Add io.js as a dependency in recipe with `RDEPENDS`.

Latest version:

```
	RDEPENDS_${PN} += "iojs"
```

### `npm install` buildable recipes

Inherit `npm` or `npm-install` build task classes in your recipe.

Bitbake classes 
===============

`meta-iojs` layer adds two new classes: `npm` and `npm-install`.

## `npm` class

`npm` defines the `oe_runnpm` command which will call cross-compiling `npm`.

For example:

```
  inherit npm
   
  do_install() {
	oe_runnpm install     # Installs dependencies defined in package.json 
  }
```

### Variables
	  
 * `NPM_FLAGS`: Extra command line arguments for `npm` calls made by `oe_runnpm()`
 * `NPM_ARCH`: Override npm target architecture (defaults to `TARGET_ARCH`)
	  
## `npm-install` class

`npm-install` class inherits `npm` class and adds following build tasks (listed in their run order):

  * `npm_install`: runs `npm install` in source directory
  * `npm_shrinkwrap`: runs `npm shrinkwrap` in source directory
  * `npm_dedupe`: runs `npm dedupe` in source directory

You can disable one or more of these build tasks in the recipe with `do_<taskname>[noexec] = "1"`:

```
  do_npm_shrinkwrap[noexec] = "1"
```

### Variables

 * `NPM_INSTALL_FLAGS`: Extra command line arguments for `npm` calls made in `npm_install` task 
 * `NPM_INSTALL`: Parameters for `npm install` command (such as specific package names)
