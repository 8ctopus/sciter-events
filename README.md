# sciter events

This is a [sciter.js](https://sciter.com/) events demo.

## get started

- git clone the repository
- run `install.bat` to download the latest sciter binaries and library
- start `scapp.exe`
- to refresh the app after changes to the html/css click `F5`

## debug app

- start `inspector.exe`
- inside the `scapp.exe` window click `CTRL + SHIFT + I` to connect to the inspector
- click `CTRL + SHIFT + left click` to inspect an element
- note: the clock component forces the DOM to refresh every second, remove the component if you want to inspect in peace.

# event handlers

[https://sciter.com/event-handling-in-sciter/](https://sciter.com/event-handling-in-sciter/)

## direct element event handlers

Element event handlers are the ones that are attached to particular elements explicitly. **The DOM element must exist for the event handler to attach to it**.
- `element.on(“eventname“, function(event) {…})`
    jQuery like form of event subscription. It matches `addEventListener()` functionality but is less verbose.  And it also allows to subscribe to events in capturing phase by prepending ^ to eventname.
- `element.addEventListener(eventName, handler [,options])`
    This is standard HTML5 way of attaching event handlers.
- `element.oneventname = function(event) {…}`
    Primitive way and not recommended because of its limitations such just one click handler can be attached to any element.

## group (a.k.a. filtered) event handlers

`element.on(“eventname“, “css selector“, function(event, matchedElement) {…})`
    - works on all elements matching css selector
    - Elements added to the DOM after the event handler was added are also tracked.

## class component event handlers

This group of event handlers is strictly Sciter specific and is used in class based UI components.

`[“on eventname at css-selector“](event, matchedChild) {}`

## event propagation

The event propagation is bidirectional, from the window to the event target and back. This propagation can be divided into three phases:

1. capture phase : from the window to the event target parent
2. target phase : the event target itself
3. bubble phase : from the event target parent back to the window

[https://towardsdev.com/event-propagation-in-javascript-4478852695cf](https://towardsdev.com/event-propagation-in-javascript-4478852695cf)
[https://javascript.info/bubbling-and-capturing](https://javascript.info/bubbling-and-capturing)

### event capturing

The event moves from the outermost element to the target element. It is rarely used in real code.

### event bubbling

When an event happens on an element, it first runs the handlers on it, then on its parent, then all the way up on other ancestors.

But any handler may decide that the event has been fully processed and stop the bubbling. The method for it is `event.stopPropagation()`. Don’t stop bubbling without a need!

If an element has multiple event handlers on a single event, then even if one of them stops the bubbling, the other ones still execute. In this case use `event.stopImmediatePropagation()`.
