# api documentation for  [gulp-size (v2.1.0)](https://github.com/sindresorhus/gulp-size)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-size.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-size) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-size.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-size)
#### Display the size of your project

[![NPM](https://nodei.co/npm/gulp-size.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/gulp-size)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-size/build/screenCapture.buildCi.browser.apidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-size/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-size/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-size/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Sindre Sorhus",
        "url": "sindresorhus.com"
    },
    "bugs": {
        "url": "https://github.com/sindresorhus/gulp-size/issues"
    },
    "dependencies": {
        "chalk": "^1.0.0",
        "gulp-util": "^3.0.0",
        "gzip-size": "^3.0.0",
        "object-assign": "^4.0.1",
        "pretty-bytes": "^3.0.1",
        "stream-counter": "^1.0.0",
        "through2": "^2.0.0"
    },
    "description": "Display the size of your project",
    "devDependencies": {
        "mocha": "*",
        "xo": "*"
    },
    "directories": {},
    "dist": {
        "shasum": "1c2b64f17f9071d5abd99d154b7b3481f8fba128",
        "tarball": "https://registry.npmjs.org/gulp-size/-/gulp-size-2.1.0.tgz"
    },
    "engines": {
        "node": ">=0.12.0"
    },
    "files": [
        "index.js"
    ],
    "gitHead": "1303830e9fc48e5f6787ddbfee232f1d956b2e35",
    "homepage": "https://github.com/sindresorhus/gulp-size",
    "keywords": [
        "gulpplugin",
        "filesize",
        "file",
        "size",
        "log",
        "measure",
        "inspect",
        "debug",
        "gzip"
    ],
    "license": "MIT",
    "maintainers": [
        {
            "name": "sindresorhus"
        }
    ],
    "name": "gulp-size",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git+https://github.com/sindresorhus/gulp-size.git"
    },
    "scripts": {
        "test": "xo && mocha"
    },
    "version": "2.1.0",
    "xo": {
        "env": [
            "node",
            "mocha"
        ]
    }
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-size](#apidoc.module.gulp-size)
1.  [function <span class="apidocSignatureSpan"></span>gulp-size (opts)](#apidoc.element.gulp-size.gulp-size)
1.  [function <span class="apidocSignatureSpan">gulp-size.</span>toString ()](#apidoc.element.gulp-size.toString)



# <a name="apidoc.module.gulp-size"></a>[module gulp-size](#apidoc.module.gulp-size)

#### <a name="apidoc.element.gulp-size.gulp-size"></a>[function <span class="apidocSignatureSpan"></span>gulp-size (opts)](#apidoc.element.gulp-size.gulp-size)
- description and source-code
```javascript
gulp-size = function (opts) {
	opts = objectAssign({
		pretty: true,
		showTotal: true
	}, opts);

	var totalSize = 0;
	var fileCount = 0;

	function log(what, size) {
		var title = opts.title;
		title = title ? chalk.cyan(title) + ' ' : '';
		size = opts.pretty ? prettyBytes(size) : (size + ' B');
		gutil.log(title + what + ' ' + chalk.magenta(size) + (opts.gzip ? chalk.gray(' (gzipped)') : ''));
	}

	return through.obj(function (file, enc, cb) {
		if (file.isNull()) {
			cb(null, file);
			return;
		}

		var finish = function (err, size) {
			if (err) {
				cb(new gutil.PluginError('gulp-size', err));
				return;
			}

			totalSize += size;

			if (opts.showFiles === true && size > 0) {
				log(chalk.blue(file.relative), size);
			}

			fileCount++;
			cb(null, file);
		};

		if (file.isStream()) {
			if (opts.gzip) {
				file.contents.pipe(gzipSize.stream())
					.on('error', finish)
					.on('end', function () {
						finish(null, this.gzipSize);
					});
			} else {
				file.contents.pipe(new StreamCounter())
					.on('error', finish)
					.on('finish', function () {
						finish(null, this.bytes);
					});
			}

			return;
		}

		if (opts.gzip) {
			gzipSize(file.contents, finish);
		} else {
			finish(null, file.contents.length);
		}
	}, function (cb) {
		this.size = totalSize;
		this.prettySize = prettyBytes(totalSize);

		if (!(fileCount === 1 && opts.showFiles) && totalSize > 0 && fileCount > 0 && opts.showTotal) {
			log(chalk.green('all files'), totalSize);
		}

		cb();
	});
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.gulp-size.toString"></a>[function <span class="apidocSignatureSpan">gulp-size.</span>toString ()](#apidoc.element.gulp-size.toString)
- description and source-code
```javascript
toString = function () {
    return toString;
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
