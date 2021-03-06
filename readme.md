## What is this?
The DOM's _document.registerElement_ and _customElements.define_ require you to include a hyphen in the node name to prevent conflict. I refused to accept this rule and this is the result. Think before using this.

* document.defineElement is a polyfill for document.registerElement modified to allow elements without a hyphen (-) 
* document.defineElement works even in browsers that do not implement document.registerElement.
* document.defineElement is renamed to prevent conflict with document.registerElement.
* document.defineElement lets you overwrite native DOM node behaviors. 
* document.defineElement can be used to create dynamic and interactive HTML node types.
* document.defineElement is *deprecated in 2018* so you can use _nativeElements.define_ (also to prevent conflict)
* nativeElements.define works just like customElements.define

## Simple clock using nativeElements.define()
The following custom &lt;clock&gt; element will show the current time and update it every second like a clock. In this example we are not using a hyphen in the node name. All we need to show a clock in our custom interface framework thing is put ``` <clock></clock> ``` somewhere.

```js
class HTMLSimpleClockElement extends HTMLSpanElement {
	createdCallback() {
   		var self = this
        this.interval = setInterval(function() {
        	self.innerText = new Date(new Date().getTime()).toLocaleTimeString()
        }, this.getAttribute('refresh') || 1000)
        console.log('created!')
	}
	connectedCallback() {
		console.log('appended!')
	}
	constructor() {
		console.log('construct!')
		super()
	}		    
}
//var Timer = document.defineElement('timer', HTMLSimpleClockElement)
nativeElements.define('timer', HTMLSimpleClockElement)
``` 

```markup
<body>
    <!-- a clock refreshing every 500ms -->
	<clock refresh="500"></clock>
</body>
``` 

## document.defineElement()
Just like _document.registerElement_, your new HTML element class supports the following (optional) callbacks. This should be pretty straightforward.
This is how we did things with registerElement before it was deprecated.

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

document.registerElement('custom-element', HTMLSomeCustomElement)
document.defineElement('custom-element', HTMLSomeCustomElement) 
/* now every <custom-element></custom-element> will be an instanceof HTMLSomeCustomElement */
```

```js
/* or like this */
var Custom = document.registerElement('custom-element', HTMLSomeCustomElement)
var Custom = document.defineElement('custom-element', HTMLSomeCustomElement)
document.body.appendChild(new Custom())
``` 

```js
/* or simply this works as well */
document.createElement('custom-element')
``` 

## Simple clock using defineElement
The following custom &lt;clock&gt; element will show the current time and update it every second like a clock. In this example we are not using a hyphen in the node name. All we need to show a clock in our custom interface framework thing is put ``` <clock></clock> ``` somewhere.

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

document.defineElement('clock', HTMLSimpleClockElement)
``` 

```markup
<body>
	<clock></clock>
</body>
``` 

## Ideas
I am not very imaginative. Sorry.

* create an ``` <include></include> ``` element that fetches remote content and renders it.
* design a ``` <chat></chat> ``` element that automatically connects to your WebSocket server
* something with ``` <user></user> ``` or ``` <like-button></like-button> ``` 

## Bad ideas

* completely overwrite the ``` <body> ``` element and break things
* make iframes or script tags stop working

