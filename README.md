# Schwifty.js
> Instant page loads for static sites.


## How it works
Schwifty preloads and caches same-origin HTML documents, using an asynchronous request.


## Usage
Once you create a new instance of Schwifty, it will automatically start preloading and caching same-origin links in the current document.

```javascript
import Schwifty from 'Schwifty';
new Schwifty();
```


## Options

### options.onload
- Type: `function`
- Default: `null`

This is the callback that runs when a new page is rendered. Use it to re-run javascript on each page load.

```javascript
function doStuff() {
	// Do stuff
}

new Schwifty({
	onload: doStuff // Called once the new page is rendered
})
```

This parameter also accepts an array of functions.

```javascript
new Schwifty({
	onload: [
		doStuff,
		doMoreStuff,
		doEvenMoreStuff
	]
})
```


### options.selector
- Type: `string`
- Default: `a[href^="${window.location.origin}"]:not([data-no-schwifty]), a[href^="/"]:not([data-no-schwifty])`

This is the selector passed to `document.querySelectorAll`, which returns valid links for preloading.


### options.cacheLimit
- Type: `number`
- Default: `85`

This is the maximum number of pages allowed to be preloaded in the cache. Once the limit is reached, Schwifty dumps the oldest entries as new documents are preloaded.


### options.preserveScroll
- Type: `boolean`
- Default: `false`

This tells Schwifty whether or not to preserve the scroll position on page load.


### options.preserveAttributes
- Type: `boolean` or `object`
- Default: `null`

This tells Schwifty whether or not to preserve attributes on top-level DOM elements (`documentElement`, `head`, and `body`).

```javascript
preserveAttributes: {
	documentElement: true,
	body: true,
	head: false
}
// Or
preserveAttributes: true
```


### options.transitioningAttribute
- Type: `string`
- Default: `data-schwifty`

This is the attribute added to the `documentElement` during a page transition.

```javascript
transitioningAttribute: 'data-isTransitioning'
```
```html
<!-- Before page transition -->
<html>...</html>

<!-- During page transition -->
<html data-isTransitioning="">...</html>

<!-- After page transition -->
<html>...</html>
```

## Known issues
- The desktop and iOS versionos of Firefox prerender Reader View once a page is navigated. Schwifty isn't