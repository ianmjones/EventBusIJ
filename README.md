# EventBusIJ #

A simple Event Bus for Desktop and Web Edition Real Studio projects.

Events can be raised for any listener to handle. Messages are sent application wide in a console or desktop application, but are scoped to the current session in web applications.

## How To Use ##

Add both the **EventBusIJ** module and the **EventHandlerIJ** interface to your project.

**EventBusIJ** extends Object, giving you access to the *AddEventHandlerIJ*, *RemoveEventHandlerIJ* and *RaiseEventIJ* methods on any object.

Use *AddEventHandlerIJ* to register an object (e.g. Window, Page, Class) as a listener for a type of event:

	me.AddEventHandlerIJ("HelloMsgBox")

The object should implement the **EventHandlerIJ** interface, which means it must have a *HandleEventIJ* method [^1].

	Function HandleEventIJ(EventType As String, Details As Variant) As Boolean
	  // Part of the EventHandlerIJ interface.
	  
	  Select Case EventType
	  Case "HelloMsgBox"
	    MsgBox "HelloMsgBoxEventController caught the ""HelloMsgBox"" event."
	  Case Else
	    // This should never happen.
	    MsgBox "Didn't handle EventType '" + EventType + "'."
	  End Select
	End Function


Then use *RaiseEventIJ* from any object to notify listeners for the event type that something has happened. Use either:

	me.RaiseEventIJ("HelloMsgBox")

or

	me.RaiseEventIJ("HelloMsgBox", Details)

where *Details* can be any value or object of any type (including dictionaries and arrays of objects or values) that the listener might need to use.

When an object goes away for any reason it's a good idea to tell **EventBusIJ** that the listener isn't interested in the event any longer...

	me.RemoveEventHandlerIJ("HelloMsgBox")

Using *RemoveEventHandlerIJ* isn't strictly necessary as **EventBusIJ** uses weak links and cleans out references to missing listeners after every few events have been processed. However you'll save memory and cycles if you clean up after yourself. Just stick a call to *RemoveEventHandlerIJ* in either the *Hidden*, *Close* or *Destructor* events as required.


[^1]: This is by design, please don't ask for the ability to specify a handler function as **EventBusIJ** is intentionally simple, having one common handler function specified by an interface makes mistakes a lot less likely.  
A future version may enforce this rule, by making the *AddEventHandlerIJ* and *RemoveEventHandlerIJ* methods only available to objects implementing the **EventHandlerIJ** interface.


## Author ##

Ian M. Jones  
http://www.ianmjones.com  
mailto:ian@ianmjones.com  


## License ##

Standard MIT license (a.k.a. do what you like with it except claim it as your own)...

Copyright (c) 2011 Ian M. Jones, IMiJ Ltd

Permission is hereby granted, free of charge, to any person
obtaining a copy of this software and associated documentation
files (the "Software"), to deal in the Software without
restriction, including without limitation the rights to use,
copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following
conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.


## Version History ##

1.0 2012-05-28

* Initial public release.

--- EOF ---