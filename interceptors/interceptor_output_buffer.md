# Interceptor Output Buffer

Every interceptor has the following methods that enable you to add content to an output buffer that will gracefully be outputted for you at the end of the event.  These methods can also be found in the `buffer` argument that is passed to each intercepting method.

* `clearBuffer():void`
* `appendToBuffer(string):void`
* `getBufferString():string`
* `getBufferObject():coldbox.system.core.util.RequestBuffer`

The buffer is unique per interception point but available to the entire chain of execution within an interception point. Once the interception point is executed, the interceptor service will check to see if the output buffer has content, if it does it will advice to write the output to the ColdFusion output stream. This way, you can produce output very cleanly from your interception points, without adding any messy-encapsulation breaking `output=true` tags to your interceptors. (**BAD PRACTICE**). This is an elegant solution that can work for both core and custom interception points.

```js
// Using methods, meaning you inherited from Interceptor or registered at configuration time.
function preRender( event, interceptData, buffer ){
	//clear all of it first, just in case.
	clearBuffer();
	//Append to buffer
	appendToBuffer( '<h1>This software is copyright by Funky Joe!</h1>' );	
}
```

> **Hint** Each execution point will have its own clean buffer to work with. As each interception point has finalized execution, the output buffer is flushed, only if content exists. 

## Output Buffer Argument

Here are some examples using the `buffer` argument:

```js
function onSidebar(event, interceptData, buffer){
	savecontent variable="local.data"{
		/// HTML HERE
	}

	arguments.buffer.append( local.data );
}

// using argument
function preRender(event,interceptData,buffer){
	//clear all of it first, just in case.
	arguments.buffer.clear();
	//Append to buffer
	arguments.buffer.append('<h1>This software is copyright by Funky Joe!</h1>');	
}
```

Here are some common methods on the buffer object:

* append(str) Append strings to the buffer
* clear() Clear the entire buffer
* length() Get size of the buffer
* getString() Get the entire string in the buffer
* getBufferObject() Get the actual java String Builder object
