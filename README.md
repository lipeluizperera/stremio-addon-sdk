# stremio-addon-sdk

A NodeJS SDK for making and publishing Stremio add-ons

This can publish an add-on via HTTP(s) or IPFS


## Example

**This arbitrary example creates an add-on that provides a stream for Big Buck Bunny and outputs a HTTP address where you can access it**

```javascript
#!/usr/bin/env node

const addonSDK = require('stremio-addon-sdk')

const addon = new addonSDK({
	id: 'org.myexampleaddon',
	version: '1.0.0',

	name: 'simple example',

	// Properties that determine when Stremio picks this add-on
	// this means your add-on will be used for streams of the type movie
	resources: ['stream'],
	types: ['movie'],
})

// takes function(type, id, cb)
addon.defineStreamHandler(function(type, id, cb) {
	if (type === 'movie' && id === 'tt1254207') {
		// serve one stream to big buck bunny
		// return addonSDK.Stream({ url: '...' })
		const stream = { url: 'http://distribution.bbb3d.renderfarming.net/video/mp4/bbb_sunflower_1080p_30fps_normal.mp4' }
		cb(null, { streams: [stream] })
	} else {
		// otherwise return no streams
		cb(null, { streams: [] })
	}
})

addon.run()

```

Save this as `addon.js` and run it:

```bash
node ./addon.js
```

It will output a URL that you can paste inside the Stremio add-on UI to try the add-on

## Documentation

### `const addonSDK = require('stremio-addon-sdk')`

Imports the SDK

### `var addon = new addonSDK(manifest)`

Creates a new ready-to-publish add-on with a given manifest. 

[Manifest is defined here.](docs/api/manifest.md)

### `addon.defineCatalogHandler(function handler(type, id, cb) { })`

### `addon.defineMetaHandler(function handler(type, id, cb) { })`

### `addon.defineStreamHandler(function handler(type, id, cb) { })`

### `addon.run()`

TODO describe handler

The JSON format of the response to these resources is described [here]().


consider `addon.precache()`
