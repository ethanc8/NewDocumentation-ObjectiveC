# Classes
## Basics of Classes

<span id="index-classes"></span>

A **class** in Objective-C is a *type of object*, much like a structure definition in C except that in addition to variables, a class has code – method implementations – associated with it. When you create an *instance* of a class, also known as an *object*, memory for each of its variables is allocated, including a pointer to the class definition itself, which tells the Objective-C runtime where to find the method code, among other things. Whenever an object is sent a message, the runtime finds this code and executes it, using the variable values that are set for this object.

<span id="Inheritance"></span>

### 2.3.1 Inheritance

<span id="index-inheritance"></span>

Most of the programmer’s time is spent defining classes. Inheritance helps reduce coding time by providing a convenient way of reusing code. For example, the `NSButton` class defines data (or instance variables) and methods to create button objects of a certain type, so a subclass of `NSButton` could be produced to create buttons of another type - which may perhaps have a different border colour. Equally `NSTextField` can be used to define a subclass that perhaps draws a different border, by reusing definitions and data in the superclass.

Inheritance places all classes in a logical hierarchy or tree structure that may have the `NSObject` class at its root. (The root object may be changed by the developer; in GNUstep it is `NSObject`, but in “plain” Objective-C it is a class called “`Object`” supplied with the runtime.) All classes may have subclasses, and all except the root class do have superclasses. When a class object creates a new instance, the new object holds the data for its class, superclass, and superclasses extending to the root class (typically `NSObject`). Additional data may be added to classes so as to provide specific functions and application logic.

When a new object is created, it is allocated memory space and its data in the form of its instance variables are initialised. Every object has at least one instance variable (inherited from `NSObject`) called `isa`, which is initialized to refer to the object’s class. Through this reference, access is also afforded to classes in the object’s inheritance path.

In terms of source code, an Objective-C class definition has an:

-   *interface* declaring instance variables, methods and the superclass name; and an
-   *implementation* that defines the class in terms of operational code that implements the methods.

Typically these entities are confined to separate files with `.h` and `.m` extensions for Interface and Implementation files, respectively. However they may be merged into one file, and a single file may implement multiple classes.

<span id="Inheritance-of-Methods"></span>

### 2.3.2 Inheritance of Methods

<span id="index-inheriting-methods"></span>

Each new class inherits methods and instance variables from another class. This results in a class hierarchy with the root class at the core, and every class (except the root) has a superclass as its parent, and all classes may have numerous subclasses as their children. Each class therefore is a refinement of its superclass(es).

<span id="Overriding-Methods"></span>

### 2.3.3 Overriding Methods

<span id="index-overriding-methods"></span>

Objects may access methods defined for their class, superclass, superclass’ superclass, extending to the root class. Classes may be defined with methods that overwrite their namesakes in ancestor classes. These new methods are then inherited by subclasses, but other methods in the new class can locate the overridden methods. Additionally redefined methods may include overridden methods.

<span id="Abstract-Classes"></span>

### 2.3.4 Abstract Classes

<span id="index-abstract-class"></span> <span id="index-class_002c-abstract"></span>

Abstract classes or abstract superclasses such as `NSObject` define methods and instance variables used by multiple subclasses. Their purpose is to reduce the development effort required to create subclasses and application structures. When we get technical, we make a distinction between a pure abstract class whose methods are defined but instance variables are not, and a semi-abstract class where instance variables are defined).

An abstract class is not expected to actually produce functional instances since crucial parts of the code are expected to be provided by subclasses. In practice, abstract classes may either stub out key methods with no-op implementations, or leave them unimplemented entirely. In the latter case, the compiler will produce a warning (but not an error).

Abstract classes reduce the development effort required to create subclasses and application structures.

<span id="Class-Clusters"></span>

### 2.3.5 Class Clusters

<span id="index-class-cluster"></span> <span id="index-cluster_002c-classes"></span>

A class cluster is an abstract base class, and a group of private, concrete subclasses. It is used to hide implementation details from the programmer (who is only allowed to use the interface provided by the abstract class), so that the actual design can be modified (probably optimised) at a later date, without breaking any code that uses the cluster.

Consider a scenario where it is necessary to create a class hierarchy to define objects holding different types including **chars**, **ints**, **shorts**, **longs**, **floats** and **doubles**. Of course, different types could be defined in the same class since it is possible to **cast** or **change** them from one to the next. Their allocated storage differs, however, so it would be inefficient to bundle them in the same class and to convert them in this way.

The solution to this problem is to use a class cluster: define an abstract superclass that specifies and declares components for subclasses, but does not declare instance variables. Rather this declaration is left to its subclasses, which share the **programmatic interface** that is declared by the abstract superclass.

When you create an object using a cluster interface, you are given an object of another class - from a concrete class in the cluster.

## `NSObject`: The Root Class

<span id="index-NSObject"></span> <span id="index-root-class"></span> <span id="index-class_002c-root"></span>

In GNUstep, `NSObject` is a root class that provides a base implementation for all objects, their interactions, and their integration in the run-time system. `NSObject` defines the `isa` instance variable that connects every object with its class.

In other Objective-C environments besides GNUstep, `NSObject` will be replaced by a different class. In many cases this will be a default class provided with the Objective-C runtime. In the GNU runtime for example, the base class is called `Object`. Usually base classes define a similar set of methods to what is described here for `NSObject`, however there are variations.

The most basic functions associated with the `NSObject` class (and inherited by all subclasses) are the following:

-   allocate instances
-   connect instances to their classes

In addition, `NSObject` supports the following functionality:

-   initialize instances
-   deallocate instances
-   compare self with another object
-   archive self
-   perform methods selected at run-time
-   provide reflective information at runtime to queries about declared methods
-   provide reflective information at runtime to queries about position in the inheritance hierarchy
-   forward messages to other objects.

<span id="The-NSObject-Protocol"></span>

### 2.4.1 The NSObject Protocol

In fact, the `NSObject` class is a bit more complicated than just described. In reality, its method declarations are split into two components: essential and ancillary. The essential methods are those that are needed by *any* root class in the GNUstep/Objective-C environment. They are declared in an “`NSObject` protocol” which should be implemented by any other root class you define (see [Protocols](Classes.html)). The ancillary methods are those specific to the `NSObject` class itself but need not be implemented by any other root class. It is not important to know which methods are of which type unless you actually intend to write an alternative root class, something that is rarely done.

<span id="Static-Typing"></span>

## 2.5 Static Typing

<span id="index-static-typing"></span>

Recall that the `id` type may be used to refer to any class of object. While this provides for great runtime flexibility (so that, for example, a generic `List` class may contain objcts of any instance), it prevents the compiler from checking whether objects implement the messages you send them. To allow type checking to take place, Objective-C therefore also allows you to use class names as variable types in code. In the following example, type checking verifies that the `myString` object is an appropriate type.

     // compiler verifies, if anObject's type is known, that it is an NSString:
    NSString *myString = anObject;
     // now, compiler verifies that NSString declares an int 'length' method:
    int len = [myString length];

Note that objects are declared as pointers, unlike when `id` is used. This is because the pointer operator is implicit for `id`. Also, when the compiler performs type checking, a subclass is always permissible where any ancestor class is expected, but not vice-versa.

<span id="Type-Introspection"></span>

### 2.5.1 Type Introspection

Static typing is not always appropriate. For example, you may wish to store objects of multiple types within a list or other container structure. In these situations, you can still perform type-checking manually if you need to send an untyped object a particular message. The `isMemberOfClass:` method defined in the `NSObject` class verifies that the receiver is of a specific class:

    if ([namedObject isMemberOfClass: specificClass] == YES)
      {
        // code here
      }

The test will return false if the object is a member of a subclass of the specific class given - an exact match is required. If you are merely interested in whether a given object *descends* from a particular class, the `isKindOfClass:` method can be used instead:

    if ([namedObject isKindOfClass: specificClass] == YES)
      {
        // code here
      }

There are other ways of determining whether an object responds to a particular method, as will be discussed in [Advanced Messaging](Advanced-Messaging.html).

<span id="Referring-to-Instance-Variables"></span>

### 2.5.2 Referring to Instance Variables

<span id="index-instance-variables_002c-referring-to"></span>

As you will see later, classes may define some or all of their instance variables to be *public* if they wish. This means that any other object or code block can access them using the standard “`->`” structure access operator from C. For this to work, the object must be statically typed (not referred to by an `id` variable).

       Bar *bar = [foo getBar];
       int c = bar->value * 2;   // 'value' is an instance variable

In general, direct instance variable access from outside of a class is not recommended programming practice, aside from in exceptional cases where performance is at a premium. Instead, you should define special methods called *accessors* that provide the ability to retrieve or set instance variables if necessary:

    - (int) value
    {
        return value;
    }

    - (void) setValue: (int) newValue
    {
        value = newValue;
    }

While it is not shown here, accessors may perform arbitrary operations before returning or setting internal variable values, and there need not even be a direct correspondence between the two. Using accessor methods consistently allows this to take place when necessary for implementation reasons without external code being aware of it. This property of *encapsulation* makes large code bases easier to maintain.

<span id="Working-with-Class-Objects"></span>

## 2.6 Working with Class Objects

Classes themselves are maintained internally as objects in their own right in Objective-C, however they do not possess the instance variables defined by the classes they represent, and they cannot be created or destroyed by user code. They do respond to class methods, as in the following:

    id result = [SomeClassName doSomething];

Classes respond to the class methods their class defines, as well as those defined by their superclasses. However, it is not allowed to override an inherited class method.

You may obtain the class object corresponding to an instance object at runtime by a method call; the class object is an instance of the “`Class`” class.

      // all of these assign the same value
    id stringClass1 = [stringObject class];
    Class stringClass2 = [stringObject class];
    id stringClass3 = [NSString class];

Classes may also define a version number (by overriding that defined in `NSObject`):

`int versionNumber = [NSString version];`

This facility allows developers to access the benefits of versioning for classes if they so choose.

<span id="Locating-Classes-Dynamically"></span>

### 2.6.1 Locating Classes Dynamically

Class names are about the only names with global visibility in Objective-C. If a class name is unknown at compilation but is available as a string at run time, the GNUstep library `NSClassFromString` function may be used to return the class object:

    if ([anObject isKindOf: NSClassFromString("SomeClassName")] == YES)
      {
        // do something ...
      }

The function returns `Nil` if it is passed a string holding an invalid class name. Class names, global variables and functions (but not methods) exist in the same name space, so no two of these entities may share the same name.

<span id="Naming-Constraints-and-Conventions"></span>

## 2.7 Naming Constraints and Conventions

<span id="index-naming-constraints"></span> <span id="index-naming-conventions"></span>

The following lists the full uniqueness constraints on names in Objective-C.

-   Neither gGlobal variables nor function names may share the same name as classes, because all three entities are allocated the same (global) name space.
-   A class may define methods using the same names as those held in other classes. (See [Overriding Methods](#Objective_002dC) above.)
-   A class may define instance variables using the same names as those held in other classes.
-   A class category may have the same name as another class category.
-   An instance method and a class method may share the same name.
-   A protocol may have the same name as a class, category, or any other entity.
-   A method and an instance variable may share the same name.

There are also a number of conventions used in practice. These help to make code more readable and also help avoid naming conflicts. Conventions are particularly important since Objective-C does not have any namespace partitioning facilities like Java or other languages.

-   Class, category and protocol names begin with an uppercase letter.
-   Methods, instance variables, and variables holding instances begin with a lowercase letter.
-   Second and subsequent words in a name should begin with a capital letter, as in “ThisIsALongName”, not “Thisisalongname”. As can be seen, this makes long names more readable.
-   Classes intended to be used as libraries (Frameworks, in NeXTstep parlance) should utilize a unique two or three letter prefix. For example, the Foundation classes all begin with ’NS’, as in “NSArray, and classes in the OmniFoundation from Omni Group (a popular library for OpenStep) began with “OF”.
-   Classes and methods intended to be used only be the developers maintaining them should be prefixed by an underscore, as in “\_SomePrivateClass” or “\_somePrivateMethod”. Capitalization rules should still be followed.
-   Functions intended for global use should beging with a capital letter, and use prefixing conventions as for classes.
