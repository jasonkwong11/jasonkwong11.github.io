---
layout: post
title:  "I Have Notes from POODR: Duck Types and Inheritance"
date:   2016-11-06 12:00:01-0400
---
Welcome to IHNfP 3! This post will cover reducing costs with duck typing and acquiring behavior through inheritance. Thanks again to Sandi Metz for writing Practical Object-Oriented Design in Ruby (POODR) which is the source of this information. Also thanks to Derek Sivers for the idea of sharing notes from books. Instead of reading a book then forgetting what you just read, Sivers came up with the idea to write his notes from the book so they can be easily accessed later on. He posts these on his website for everyone to access at sivers.org/book

Duck Types

So, what are duck types? Duck types are public interfaces that are not tied any specific class: chameleons that are defined more by their behavior than by their class. These across-class interfaces add enormous flexibility to your application by replacing costly dependencies with more forgiving dependencies on messages.

Within your app, an object's type is in the eye of the beholder. Users of an object need not and should not, be concerned about its class. Class is just one way for an object to acquire a public interface; the public interface an object obtains by way of its class may be one of several that it contains. Applications may define many public interfaces that are not related to one specific class; these interfaces cut across class... It's not what an object is that matters, it's what it does.

Finding the Duck

	The key to removing the dependencies is to recognize that because a class's method serves a single purpose, it's arguments arrive wishing to collaborate to a single goal. Every argument is here for the same reason and that reason is unrelated to the argument's underlying class. 
	Avoid getting sidetracked by your knowledge of what each argument's class already does; think isntead about what the method needs. The final duck typed alternative is more abstract. It places slightly greater demands on your understanding but in return offers ease of extension. You can elicit new behavior from your application without changing any existing code. You simply turn another object into a Preparer and pass it into the Class's method. 

	The ability to tolerate ambiguity about the class of an object is the hallmark of a confident designer. Duck typing is related to polymorphism, the ability of many different objects to respond to the same message: they agree to be interchangeable from the sender's point of view. Any object implementing a polymorphic method can be substituted for any other; the sender of the message need not know or care about this substitution. Using duck typing relies on your ability to recognize the places where your application would benefit from across-class interfaces. It is relatively easy to implement a duck type; your design challenge is to notice that you need one and to abstract its interface.
	
	These methods are signs you may be able to switch to duck types: case statements that switch on class, kind_of?, is_a?, and responds_to?

Inheritance

What is inheritance? Inheritance at it's core, is a mechanism for automatic message delegation. It defines a forwarding path for not-understood messages. It creates relationships, such that, if one object cannot respond to a received message, it delegates that message to another. You don't have to write code to explicitly delegate the message, instead you define an inheritance relationship between two objects and the forwarding happens automatically. 

Inheritance is defined by creating subclasses. Messages are forward from subclass to superclass; the shared code is defined in the class hierarchy. 

Where to use Inheritance?

Well-designed inheritance hierarchies are easy to extend with new subclasses, even for programmers who know very little about the application. This ease of extension is inheritance's greatest strength. When your problem is one of needing numerous specializations of a stable, common abstraction, inheritance can be an extremely low-cost solution. 

How? Start with a concrete class. When you have mountain bikes and road bikes and they are both bikes and thus inherit much of the same from the Bike class. Use inheritance when you have highly related types that share common behavior but differ along some dimension. 

Choosing Inheritance

Obvious fact: objects receive messages. No matter how complicated the code, the receiving object ultimately handles any message in one of two ways. It either responds directly or it passes the message on to some other object for a response. Inheritance provides a way to define two objects as having a relationship such that when the first receives a message that it doesn't understand, it automatically forwards or delegates the message to the second. 

*** Ruby has single inheritance. A Superclass may have many subclasses, but each subclass is permitted only one superclass.

Creating an Abstract Superclass
	The superclass should contain all the common behavior. Each subclass will add specializations. The superclass is now an abstract class. It exists to be subclassed. It almost never makes sense to create an abstract superclass with only one subclass. Creating a hierarchy has costs; the best way to minimize these costs is to maximize your chance of getting the abstraction right before allowing subclasses to depend on it. It depends on how soon you expect a more subclasses to be needed versus how much you expect the duplication to cost.


Be Careful

If you're refactoring an existing class from concrete to abstract by pushing the concrete parts down into a new subclass you might accidentally leave remnants of concrete behavior behind. Beware when superclasses impose a requirement upon its subclasses. Subclasses may fail because they don't fulfill requirements of which they are unaware. Explicitly stating that subclasses are required to implement a message provides useful documentation for those who can be relied upon to read and useful error message for those who cannot. Create code that fails with reasonable error messages

Remember, if you have the same method in the subclass, it is overridden by the subclass. Sending super in any method passes the message up the superclass chain. Another common mistake is making a superclass that originally had a generic name for an object, but was actually slightly more specialized.

Decoupling Subclasses Using Hook Messages

Instead of allowing subclasses to know the algorithm and requiring that they send super, superclasses can instead send hook messages, ones that exist solely to provide subclasses a place to contribute information by implementing matching methods. This strategy removes knowledge of the algorithm from the subclass and returns control to the superclass. (THIS IS REALLY COOL)

