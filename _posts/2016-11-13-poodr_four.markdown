---
layout: post
title:  "I Have Notes from POODR: Sharing Role Behavior with Modules"
date:   2016-11-13 12:00:01-0400
---

Why share role behavior with modules? 

Because classical inheritance doesn't accommodate cases when you need to combine the qualities of two existing subclasses. No design technique is free and creating the most cost-effective application requires making informed tradeoffs between relative costs and likely benefits of alternatives. 

What are roles? 

Some problems require sharing behavior among otherwise unrelated objects. This common behavior is orthogonal to class; it's a role an object plays... many needed roles will be obvious now, but it's common to discover unanticipated roles as you write the code. Example of a role: The Preparer duck type from Chapter 5. Objects that implement Preparer's interface play this role. When a role needs shared behavior, you're faced with the problem of organizing the shared code. Ideally, this code would be defined in a single place but be usable by any object that wished to act as the duck type and play the role.

Modules are a way to define a named group of methods that are independent of class and can be mixed in to any object (mixins). They provide a perfect way to allow objects of different classes to play a common role using a single set of code. When an object includes a module, the methods defined therein become available via automatic delegation.  

Once you start putting code into modules and adding modules to objects, you expand the set of messages to which an object can respond and enter a new realm of design complexity. An object that directly implements fews methods might still have a very large response set including:

	1. Those it implements
	2. Those implemented in all objects above it in the hierarchy
	3. Those implemented in any module that has been added to it 
	4. Those implemented in all modules added to any object above it in the hierarchy 

Let Objects Speak for Themselves

	You shouldn't have a seperate class (like StringUtils) to manage objects. Objects should manage themselves and contain their own behavior. (Like using a Schedule class to determine if a target is schedulable) Just as strings respond to empty? and can speak for themselves Targets should respond to schedulable? the schedulable? method should be added to the interface of the Schedulable role. 

Writing the Concrete Code

	You have two decisions to make: Where should the code live? What should the code do? The simplest way to get started is to separate the two decisions and implement the schedulable? method directly in that class. Pick an arbitrary concrete class (for example, Bicycle) and implement the schedulable? method directly in that class. Once you have a version that works for Bicycle, you can refactor your way to a code arrangement that allows Schedulables to share that behavior. See figure 7.3 in the POODR. (summary: when a bicycle object calls .schedulable? method, it invokes Schedule's schedulable method. The Schedule object is newly instantiated in the attr_accessor of the Bicycle object... the code hides knowledge of who the Schedule is and what the Schedule does inside of Bicycle. Objects holding onto a Bicycle no longer need know about the existence or behavior of Schedule.)

The difference between classical inheritance and sharing code via modules. This "is-a" (inheritance) versus "behaves-like-a" (modules) difference definitely matters, each choice has distinct consequences. However the coding techniques for these two things are very similar and this similarity exists because both techniques rely on automatic message delegation. 
 
A Small Aside: Looking up Methods

 How does Ruby find the method implementation that matches a message send? 

A Gross Oversimplification: when an object receives a message, the OO language first looks in that object's class for matching method implementation. This makes perfect sense; method definitions would otherwise need to be duplicated within every instance of every class. Storing the methods known to an object inside of its class means that all instances of a class can share the same set of method definitions; definitions that need to exist in one place. 

In the case of inheritance, the search for a method process begins in the class of a receiving object. If this class does not implement the message, the search proceeds to its superclass. From here on, only superclasses matter, the search proceeds up the superclass chain, looking in one superclass after another, until it reaches the top of the hierarchy. 

A More Accurate Explanation: when a Bicycle includes Schedulable, all of the methods defined in the module become part of Bicycle's response set. The module's methods go into the method lookup path directly above methods defined in Bicycle. Including this module doesn't change Bicycle's superclass (that's still Object), but as far as method lookup is concerned it may as well have. Any messages received by an instance of MountainBike now stands a chance of being satisfied by a method defined in the Schedulable module... implication: if Bicycle implements a method that is also defined in Schedulable, Bicycle's implementation overrides Schedulable's. If Schedulable sends methods that it does not implement, instances of Mountain Bike may encounter confusing failures. 

A Very Nearly Complete Explanation: It's possible for hierarchy to contain a long chain of superclasses, each of which includes many modules. When a single class includes several different modules, the modules are place in the method lookup path in reverse order of module inclusion... the methods of the last included module are encountered first in the lookup path. You can include a module into a class using Ruby's include keyword. But it is possible to add a module's methods to a single object, using Ruby's extend keyword.

	... because extend adds the module's behavior directly to an object, extending a class with a module creates class methods in that class and extending an instance of a class with a module creates instance methods in that instance. Any object can also have ad hoc methods added directly to its own personal "Singleton class". These ad hoc methods are unique to this specific object. Each of these alternatives adds to an object's response set by placing method definitions in specific and unambiguous places along the method lookup path.

Inheriting Role Behavior

Now you can write modules that include other modules. You can write modules that override the methods defined in other modules. You can create deeply nested class inheritance hierarchies and then include these various modules at different levels of the hierarchy. You need to learn to use these in the right way by...

Recognizing the Anti-patterns:

1. An object that uses a variable with a name like "type" or "category" to determine what message to send self contains two highly related but slightly different types
	... bad because code must change every time a new type is added
	... instead, rearrange this code to use inheritance by putting the common code in abstract superclass and create subclasses for the different types (this allows you to create new subtypes by adding new subclasses.. extend the hierarchy without changing the existing code)

2. When a sending object checks the class of a receiving object to determine what message to send, you have overlooked a duck type. 
	... bad cuz code must change every time you introduce a new class of receiver
	... this role should be codified as a duck type and receivers should implement the duck type's interface. Once they do, the original object can send one single message to every receiver, confident that because each receiver plays the role it will understand the common message
	... in addition to sharing an interface ^^, duck types might also share behavior. When they do, place the shared code in a module and include that module in each class or object that plays the role.


Insist on the Abstraction
	
All of the code in an abstract superclass should apply to every class that inherits it. It should not contain code that applies to some, but not all, subclasses. This also applies to modules: the code in a module must apply to all who use it. If you can't identify the abstraction, there may not be one, and if no common abstractions exists then inheritance is not the solution for your design problem. 

Honor the Contract: Subclasses agree to a contract: they promise to be substitutable for their superclasses. This is possible only when objects behave as expected and subclasses are expected to conform to their superclasses's interface. Then you are following LSP (Liskov Substitution Principle): subtypes must be substitutable for their supertypes.

Preemptively Decouple Classes
 
Avoid writing code that requires its inheritors to send super, instead use hook messages to allow subclasses to participate while absolving them of responsibility for knowing the abstract algorithm. Inheritance by its nature adds powerful dependencies on structure and arrangement of code. Writing code that requires subclasses to send super adds an additional dependency: avoid this if you can. Hook methods solve the problem of sending super, but unfortunately only for adjacent levels of the hierarchy... So you should create shallow hierarchies.
