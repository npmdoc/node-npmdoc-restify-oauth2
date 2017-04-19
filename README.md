# npmdoc-restify-oauth2

#### api documentation for  [restify-oauth2 (v4.0.4)](https://github.com/domenic/restify-oauth2#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-restify-oauth2.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-restify-oauth2) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-restify-oauth2.svg)](https://travis-ci.org/npmdoc/node-npmdoc-restify-oauth2)

#### A simple OAuth 2 endpoint for Restify

[![NPM](https://nodei.co/npm/restify-oauth2.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/restify-oauth2)

- [https://npmdoc.github.io/node-npmdoc-restify-oauth2/build/apidoc.html](https://npmdoc.github.io/node-npmdoc-restify-oauth2/build/apidoc.html)

[![apidoc](https://npmdoc.github.io/node-npmdoc-restify-oauth2/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-restify-oauth2/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-restify-oauth2/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-restify-oauth2/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Domenic Denicola",
        "url": "http://domenic.me/"
    },
    "bugs": {
        "url": "http://github.com/domenic/restify-oauth2/issues"
    },
    "dependencies": {
        "underscore": "1.x"
    },
    "description": "A simple OAuth 2 endpoint for Restify",
    "devDependencies": {
        "api-easy": "^0.4.0",
        "chai": "^3.2.0",
        "coffee-script": "^1.9.3",
        "jshint": "^2.8.0",
        "mocha": "^2.2.5",
        "restify": "^4.0",
        "sinon": "^1.16.1",
        "sinon-chai": "^2.8.0",
        "vows": "^0.8.1"
    },
    "directories": {},
    "dist": {
        "shasum": "ebe6f853aabd5ab501a8b257896748146eafdd5d",
        "tarball": "https://registry.npmjs.org/restify-oauth2/-/restify-oauth2-4.0.4.tgz"
    },
    "gitHead": "fd3181065dc8dea483bda090ab0b9728b5ee84ea",
    "homepage": "https://github.com/domenic/restify-oauth2#readme",
    "keywords": [
        "restify",
        "oauth",
        "oauth2",
        "rest",
        "authorization",
        "authentication",
        "api"
    ],
    "license": "WTFPL",
    "main": "lib/index.js",
    "maintainers": [
        {
            "name": "domenic"
        },
        {
            "name": "gmaniac"
        }
    ],
    "name": "restify-oauth2",
    "optionalDependencies": {},
    "peerDependencies": {
        "restify": "4.x"
    },
    "repository": {
        "type": "git",
        "url": "git://github.com/domenic/restify-oauth2.git"
    },
    "scripts": {
        "lint": "jshint lib && jshint examples",
        "test": "npm run test-ropc-unit && npm run test-cc-unit && npm run test-ropc-integration && npm run test-cc-integration && npm run test-cc-with-scopes-integration",
        "test-cc-integration": "vows test/cc-integration.coffee --spec",
        "test-cc-unit": "mocha test/cc-unit.coffee --reporter spec --compilers coffee:coffee-script/register",
        "test-cc-with-scopes-integration": "vows test/cc-with-scopes-integration.coffee --spec",
        "test-ropc-integration": "vows test/ropc-integration.coffee --spec",
        "test-ropc-unit": "mocha test/ropc-unit.coffee --reporter spec --compilers coffee:coffee-script/register"
    },
    "version": "4.0.4"
}
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
