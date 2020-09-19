![The Schwifty.js Logo](/logo.png)

# Schwifty.js

> Instant page loads for static sites.


## How it works
Schwifty preloads and caches same-origin HTML documents, using an asynchronous XHTTP request.


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

This is the callback that runs when a new page is rendered. Use it to re-run Javascript on each page load.

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

This is the maximum number of pages Schwifty will preload before the cache is "full". When new pages are added to the cache, the oldest entries will be dumped to make room.


### options.preserveScroll
- Type: `boolean`
- Default: `false`

This tells Schwifty whether or not to preserve the scroll position upon loading a page.


### options.preserveAttributes
- Type: `boolean` or `object`
- Default: `false`

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

### iOS Safari (and other browsers running on top of it) won't always re-render Reader View when navigation occurs.
Schwifty emits only the standard events that occur on a page load, which usually don't trigger iOS Safari to update the content in Reader View. This can lead to a user tapping Reader View on one page, only to see the content from a page they had visited previously. The proper content is rendered only after refreshing.

Until more is known about what triggers Reader View updates, I've put a pin in trying to debug this issue.
