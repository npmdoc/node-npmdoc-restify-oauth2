# api documentation for  [restify-oauth2 (v4.0.4)](https://github.com/domenic/restify-oauth2#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-restify-oauth2.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-restify-oauth2) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-restify-oauth2.svg)](https://travis-ci.org/npmdoc/node-npmdoc-restify-oauth2)
#### A simple OAuth 2 endpoint for Restify

[![NPM](https://nodei.co/npm/restify-oauth2.png?downloads=true)](https://www.npmjs.com/package/restify-oauth2)

[![apidoc](https://npmdoc.github.io/node-npmdoc-restify-oauth2/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-restify-oauth2_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-restify-oauth2/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-restify-oauth2/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-restify-oauth2/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Domenic Denicola",
        "email": "domenic@domenicdenicola.com",
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
            "name": "domenic",
            "email": "domenic@domenicdenicola.com"
        },
        {
            "name": "gmaniac",
            "email": "grraymond101@gmail.com"
        }
    ],
    "name": "restify-oauth2",
    "optionalDependencies": {},
    "peerDependencies": {
        "restify": "4.x"
    },
    "readme": "ERROR: No README data found!",
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



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module restify-oauth2](#apidoc.module.restify-oauth2)
1.  [function <span class="apidocSignatureSpan">restify-oauth2.</span>cc (server, options)](#apidoc.element.restify-oauth2.cc)
1.  [function <span class="apidocSignatureSpan">restify-oauth2.</span>ropc (server, options)](#apidoc.element.restify-oauth2.ropc)



# <a name="apidoc.module.restify-oauth2"></a>[module restify-oauth2](#apidoc.module.restify-oauth2)

#### <a name="apidoc.element.restify-oauth2.cc"></a>[function <span class="apidocSignatureSpan">restify-oauth2.</span>cc (server, options)](#apidoc.element.restify-oauth2.cc)
- description and source-code
```javascript
function restifyOAuth2Setup(server, options) {
    if (typeof options.hooks !== "object" || options.hooks === null) {
        throw new Error("Must supply hooks.");
    }
    requiredHooks.forEach(function (hookName) {
        if (typeof options.hooks[hookName] !== "function") {
            throw new Error("Must supply " + hookName + " hook.");
        }
    });

    if (typeof options.hooks.grantScopes !== "function") {
        // By default, grant no scopes.
        options.hooks.grantScopes = function (credentials, scopesRequested, req, cb) {
            cb(null, []);
        };
    }

    options = _.defaults(options, {
        tokenEndpoint: "/token",
        wwwAuthenticateRealm: "Who goes there?",
        tokenExpirationTime: Infinity
    });

    // Allow 'tokenExpirationTime: Infinity' (like above), but translate it into 'undefined' so that
    // 'JSON.stringify' omits it entirely when we write out the response as
    // 'JSON.stringify({ expires_in: tokenExpirationTime, ... })'.
    if (options.tokenExpirationTime === Infinity) {
        options.tokenExpirationTime = undefined;
    }

    server.post(options.tokenEndpoint, function (req, res, next) {
        grantToken(req, res, next, options);
    });

    server.use(function ccOAuth2Plugin(req, res, next) {
        res.sendUnauthenticated = function (message) {
            errorSenders.authenticationRequired(res, res.send.bind(res), options, message);
        };

        res.sendUnauthorized = function (message) {
            errorSenders.insufficientAuthorization(res, res.send.bind(res), options, message);
        };

        if (req.method === "POST" && req.path() === options.tokenEndpoint) {
            // This is handled by the route installed above, so do nothing.
            next();
        } else if (req.authorization.scheme) {
            handleAuthenticatedResource(req, res, next, options);
        } else {
            // Otherwise Restify will set it by default, which gives false positives for application code.
            req.username = null;
            next();
        }
    });
}
```
- example usage
```shell
...
var restify = require("restify");
var restifyOAuth2 = require("restify-oauth2");

var server = restify.createServer({ name: "My cool server", version: "1.0.0" });
server.use(restify.authorizationParser());
server.use(restify.bodyParser({ mapParams: false }));

restifyOAuth2.cc(server, options);
// or
restifyOAuth2.ropc(server, options);
'''

Unfortunately, Restify–OAuth2 can't be a simple Restify plugin. It needs to install a route for the token
endpoint, whereas plugins simply run on every request and don't modify the server's routing table.
...
```

#### <a name="apidoc.element.restify-oauth2.ropc"></a>[function <span class="apidocSignatureSpan">restify-oauth2.</span>ropc (server, options)](#apidoc.element.restify-oauth2.ropc)
- description and source-code
```javascript
function restifyOAuth2Setup(server, options) {
    if (typeof options.hooks !== "object" || options.hooks === null) {
        throw new Error("Must supply hooks.");
    }
    requiredHooks.forEach(function (hookName) {
        if (typeof options.hooks[hookName] !== "function") {
            throw new Error("Must supply " + hookName + " hook.");
        }
    });

    if (typeof options.hooks.grantScopes !== "function") {
        // By default, grant no scopes.
        options.hooks.grantScopes = function (credentials, scopesRequested, req, cb) {
            cb(null, []);
        };
    }

    options = _.defaults(options, {
        tokenEndpoint: "/token",
        wwwAuthenticateRealm: "Who goes there?",
        tokenExpirationTime: Infinity
    });

    // Allow 'tokenExpirationTime: Infinity' (like above), but translate it into 'undefined' so that
    // 'JSON.stringify' omits it entirely when we write out the response as
    // 'JSON.stringify({ expires_in: tokenExpirationTime, ... })'.
    if (options.tokenExpirationTime === Infinity) {
        options.tokenExpirationTime = undefined;
    }

    server.post(options.tokenEndpoint, function (req, res, next) {
        grantToken(req, res, next, options);
    });

    server.use(function ccOAuth2Plugin(req, res, next) {
        res.sendUnauthenticated = function (message) {
            errorSenders.authenticationRequired(res, res.send.bind(res), options, message);
        };

        res.sendUnauthorized = function (message) {
            errorSenders.insufficientAuthorization(res, res.send.bind(res), options, message);
        };

        if (req.method === "POST" && req.path() === options.tokenEndpoint) {
            // This is handled by the route installed above, so do nothing.
            next();
        } else if (req.authorization.scheme) {
            handleAuthenticatedResource(req, res, next, options);
        } else {
            // Otherwise Restify will set it by default, which gives false positives for application code.
            req.username = null;
            next();
        }
    });
}
```
- example usage
```shell
...

var server = restify.createServer({ name: "My cool server", version: "1.0.0" });
server.use(restify.authorizationParser());
server.use(restify.bodyParser({ mapParams: false }));

restifyOAuth2.cc(server, options);
// or
restifyOAuth2.ropc(server, options);
'''

Unfortunately, Restify–OAuth2 can't be a simple Restify plugin. It needs to install a route for the token
endpoint, whereas plugins simply run on every request and don't modify the server's routing table.

## Options
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
