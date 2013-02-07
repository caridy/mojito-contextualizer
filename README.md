# Mojito Server Contextualizer Component

This compontent is responsible for attaching the
`context` information under `req.context` object
per request. It also does validation based on the
"dimensions" defined as part of the configuration.

If the component is plug within `mojito`, it will
automatically attach the `context` and `dimensions`
into the runtime object produced by `mojito-server`.

## Usage as stand alone middleware for express

```
/*jslint node:true, nomen: true*/

'use strict';

var express        = require('express'),
    contextualizer = require('mojito-contextualizer'),
    app            = express();

contextualizer({
    more: 'configs here',
    dimensions: {
        lang: {
            "en-US": null
        }
    }
});

app.use(contextualizer.lang);

// mojito will use dispatcher config to dispatch "index"
app.get('*', function (req, res, next) {
    res.send({
        server: contextualizer.config(),
        expose: contextualizer.expose(req, res)
    });
});

// listening
app.set('port', process.env.PORT || 8666);
app.listen(app.get('port'), function () {
    console.log("Server listening on port " +
        app.get('port') + " in " + app.get('env') + " mode");
});
```

## Usage within mojito

```
mojito.plug(require('mojito-contextualizer'));
mojito.contextualizer({
    foo: 'context',
    dimensions: { /* can should be coming from locator */
        lang: {
            "en-US": null
        },
        speed: {
            dialup: null,
            dsl: null
        }
    }
});
app.use(mojito.contextualizer.all);
```