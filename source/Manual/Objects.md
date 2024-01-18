# 2.2 Objects

<span id="index-objects"></span>

Object-oriented (OO) programming is based on the notion that a software system can be composed of objects that interact with each other in a manner that parallels the interaction of objects in the physical world.

This model makes it easier for the programmer to understand how software works since it makes programming more intuitive. The use of objects also makes it easier during program design: take a big problem and consider it in small pieces, the individual objects, and how they relate to each other.

Objects are like mini programs that can function on their own when requested by the program or even another object. An object can receive messages and then act on these messages to alter the state of itself (the size and position of a rectangle object in a drawing program for example).

In software an object consists of instance variables (data) that represent the state of the object, and methods (like C functions) that act on these variables in response to messages.

As a programmer creating an application or tool, all you need do is send messages to the appropriate objects rather than call functions that manipulate data as you would with a procedural program.

The syntax for sending a message to an object, as shown below, is one of the additions that Objective-C adds to ANSI C.

```objc
[objectName message]; 
```

Note the use of the square \[ \] brackets surrounding the name of the object and message.

Rather than ’calling’ one of its methods, an object is said to ’perform’ one of its methods in response to a message. The format that a message can take is discussed later in this section.

<span id="Id-and-nil"></span>

## 2.2.1 Id and nil

Objective-C defines a new type to identify an object: `id`, a type that points to an object’s data (its instance variables). The following code declares the variable ’`button`’ as an object (as opposed to ’`button`’ being declared an integer, character or some other data type).

```objc
id button;
```

When the button object is eventually created the variable name ’`button`’ will point to the object’s data, but before it is created the variable could be assigned a special value to indicate to other code that the object does not yet exist.

Objective-C defines a new keyword `nil` for this assignment, where `nil` is of type `id` with an unassigned value. In the button example, the assignment could look like this:

    id button = nil;

which assigns `nil` in the declaration of the variable.

You can then test the value of an object to determine whether the object exists, perhaps before sending the object a message. If the test fails, then the object does not exist and your code can execute an alternative statement.

    if (anObject != nil)
      ... /* send message */
    else
      ... /* do something else */

The header file `objc/objc.h` defines `id`, `nil`, and other basic types of the Objective-C language. It is automatically included in your source code when you use the compiler directive `#include <Foundation/Foundation.h>` to include the GNUstep Base class definitions.

<span id="Messages"></span>

## 2.2.2 Messages

<span id="index-messages"></span>

A message in Objective-C is the mechanism by which you pass instructions to objects. You may tell the object to do something for you, tell it to change its internal state, or ask it for information.

A message usually invokes a method, causing the receiving object to respond in some way. Objects and data are manipulated by sending messages to them. Like C-functions they have return types, but function specific to the object.

Objects respond to messages that make specific requests. Message expressions are enclosed in square brackets and include the receiver or object name and the message or method name along with any arguments.

To send a message to an object, use the syntax:

`[receiver messagename];`

where `receiver` is the object.

  

The run-time system invokes object methods that are specified by messages. For example, to invoke the display method of the mySquare object the following message is used:

`[mySquare display];`

  

Messages may include arguments that are prefixed by colons, in which case the colons are part of the message name, so the following message is used to invoke the `setFrameOrigin::` method:

`[button setFrameOrigin: 10.0 : 10.0];`

  

Labels describing arguments precede colons:

`[button setWidth: 20.0 height: 122.0];`

invokes the method named `setWidth:height:`

  

Messages that take a variable number of arguments are of the form:

`[receiver makeList: list, argOne, argTwo, argThree];`

  

A message to `nil` does NOT crash the application (while in Java messages to `null` raise exceptions); the Objective-C application does nothing.

For example:

`[nil display];`

will do nothing.

If a message to `nil` is supposed to return an object, it will return `nil`. But if the method is supposed to return a primitive type such as an `int`, then the return value of that method when invoked on `nil`, is undefined. The programmer therefore needs to avoid using the return value in this instance.

<span id="Polymorphism"></span>

## 2.2.3 Polymorphism

<span id="index-polymorphism"></span>

Polymorphism refers to the fact that two different objects may respond differently to the same message. For example when client objects receive an alike message from a server object, they may respond differently. Using Dynamic Binding, the run-time system determines which code to execute according to the object type.