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

## events

An event is a signal that something has happened. All DOM nodes generate such signals but events are not limited to DOM.

[https://javascript.info/introduction-browser-events](https://javascript.info/introduction-browser-events)

mouse
- `click` mouse left click
- `contextmenu` mouse right click
- `mouseover`,`mouseout` mouse cursor over, leaves element
- `mousedown`,`mouseup` mouse pressed, released
- `mousemove` mouse moving over element

keyboard
- `keydown`, `keyup` key pressed or released

form
- `submit` form submission
- `focus` element focused

document
- `DOMContentLoaded` DOM fully built

css
- `transitionend` CSS animation finished

## event handlers

Event handlers allow to react to events.

[https://sciter.com/event-handling-in-sciter/](https://sciter.com/event-handling-in-sciter/)

### direct element event handler

Element event handlers are attached to a particular element explicitly, which means that the **DOM element must exist for the event handler to attach to it**.
- `element.on("eventname", function(event) {…})` jQuery like form of event subscription. It matches `addEventListener()` functionality but is less verbose.  And it also allows to subscribe to events in capturing phase by prepending ^ to eventname.
- `element.addEventListener(eventName, handler [,options])` standard HTML5 event handler
- `element.oneventname = function(event) {…}` primitive way and not recommended because of its limitations such just one click handler can be attached to any element. to confirm: propagation cannot be stopped?

### group (a.k.a. filtered) event handler

`element.on("eventname", "css selector", function(event, matchedElement) {…})`
- applies to all elements matching css selector
- Elements added to the DOM after the event handler was added are also tracked

### class component event handler

This group of event handlers is strictly Sciter specific and is used in class based UI components.

`["on eventname at css-selector"](event, matchedChild) {}`

## event object

[https://github.com/8ctopus/sciter-js-sdk/blob/main/docs/md/Event.md](https://github.com/8ctopus/sciter-js-sdk/blob/main/docs/md/Event.md)

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

## send events from code

Events can also be sent from the javascript code by using the `dispatchEvent` method.

`element.dispatchEvent(new Event(typeArg [, eventInit]));`

eventInit
- `bubbles` default `false`
- `cancelable` default `false`
- `composed` default `false` whether the event will trigger listeners outside of a shadow root

```js
document.addEventListener("click", function(event) {
    console.log(event.type);
}, false);

const event = new Event("click", {
    "bubbles": true,
    "cancelable": false,
    "composed": false,
});

document.dispatchEvent(event);
```

[https://developer.mozilla.org/en-US/docs/Web/API/Event/Event](https://developer.mozilla.org/en-US/docs/Web/API/Event/Event)

### custom events

```js
element.addEventListener("build", function(event) {
    console.log(event.detail);
}, false);

const event = new CustomEvent("build", { detail: "custom details" });

element.dispatchEvent(event);
```

[https://developer.mozilla.org/en-US/docs/Web/Events/Creating_and_triggering_events#creating_custom_events](https://developer.mozilla.org/en-US/docs/Web/Events/Creating_and_triggering_events#creating_custom_events)
