## What is this?
The DOM's _document.registerElement_ requires you to include a hyphen in the node name to prevent conflict. I *refuse* to accept this rule and this is the result.

* document.defineElement is a polyfill for document.registerElement modified to allow elements without a hyphen (-) 
* document.defineElement works even in browsers that do not implement document.registerElement.
* document.defineElement is renamed to prevent conflict with document.registerElement.
* document.defineElement lets you overwrite *all* existing DOM node behaviors. 
* document.defineElement can be used to create dynamic and interactive HTML node types.

## document.defineElement()
Just like _document.registerElement_, your new class supports the following (optional) callbacks. This should be pretty straightforward.

```js 
class HTMLSomeCustomElement extends HTMLElement {
	/* Fires when an instance of the element is created. */
    createdCallback() {
    	/* */
    }

    /* Fires when an instance was inserted into the document. */
    attachedCallback() {
        /* */
    }

    /* Fires when an instance was removed from the document. */
    detachedCallback() {
    	/* */
    }

    /* Fires when an attribute was added, removed, or updated. */
    attributeChangedCallback(attr, oldVal, newVal) {
    	/* */
    }
}

document.defineElement('custom-element', HTMLSomeCustomElement) 
/* now every <custom-element></custom-element> will be an instanceof HTMLSomeCustomElement */
```

```js
/* or you can do this too */
var Custom = document.defineElement('custom-element', HTMLSomeCustomElement)
document.body.appendChild(new Custom())
``` 

```js
/* or simply this works as well */
document.createElement('custom-element')
``` 

## Simple clock, an actual example
The following custom element will show the current time and update it every second like a clock. In this example we are not using a hyphen in the node name. All we need to show a clock in our custom interface framework thing is put ```markup <clock></clock> ``` somewhere.

```js
class HTMLSimpleClockElement extends HTMLSpanElement {

	createdCallback() {
   		var self = this
        /* we could do something with this.getAttribute() for instance set interval */
        this.interval = setInterval(function() {
        	self.innerText = new Date(new Date().getTime()).toLocaleTimeString()
        }, 1000)
	}
    
}

document.registerElement('clock', HTMLSimpleClockElement)
``` 

```markup
<body>
	<clock></clock>
</body>
``` 

## Ideas
I am not very imaginative. Sorry.

* create an ```markup <include></include> ``` element that fetches remote content and renders it.
* design a ```markup <chat></chat> ``` element that automatically connects to your WebSocket server
* something with ```markup <user></user> ``` or ```markup <like-button></like-button> ``` 

## Bad ideas

* completely overwrite the ```markup <body> ``` element and break things
* make iframes or script tags stop working

