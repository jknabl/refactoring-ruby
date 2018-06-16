# Chapter 3: Bad Smells in Code

* _Code smells_: patterns we've noticed over the years that frequently indicate bad, difficult code.

## Duplicated code

* If you find the same code structure in > 1 place, it is certain the code would be better if you unified all occurrences.

* When you ahve the same expression in two methods of the same class
  * `Extract Method`, invoke new method from both places.

* When you have same expression in two sibiling subclasses
  * `Extract Method` in both classes
  * `Pull Up Method` (to superclass)
  * If code is similar but not the same, first use `Extract Method` to separate similar bits from different bits.
    * Then use `Form Template Method`
  * If methods do same thing with different algorithms, use `Substitute Algorithm` with the clearer algo.
  * If duplication is in the middle of the method, use `Extract Surrounding Method`

* If there is dupe code in two unrelated classes, you could use `Extract Class` or `Extract Module` on one class and use the new component in the other. 
  * Could be the case that the method is better suited in a third class that should be referred to by both original classes (e.g. both classes are collaborating to do some other thing)

## Long Method

* All the payoffs if indirection (explanation, sharing, and choosing) are supported by small methods.
* The longer a method is, the more difficult it is to understand. 
* Well-named, small methods == good
* Be aggressive about decomposing methods.
* 99% of the time you only need `Extract Method` to solve long method. 
* If you have a long method with many parameters/temporary variables:
  * `Replace Temp with Query` or `Replace Temp with Chain` can eliminate temps.
  * Long param lists can be fixed with `Introduce Parameter Object` or `Preserve Whole Object`.
  * If after the above refactorings the method is still unwieldy, maybe you need an entirely new object (`Replace Method with Method Object`)
* Conditionals and loops are also signals you should extract.
  * `Decompose Conditional` eliminates conditionals.
  * `Collection Closure Methods` can replace loops.
    * Use `Extract Method` on the call to the closure method/the closure itself.

## Large Class

* Many instance variables are a sign a class is doing too much.
  * `Extract Class` to bundle some of the variables.
    * Or `Extract Subclass` if relevant.
    * If the component doesn't make sense as a delegate, use `Extract Module`
* If a class doesn't use all of its ivars all the time, you can `Extract Class/Module/Subclass` for each use combination. 
* A useful trick for breaking up large classes is to determine how clients are using the class. Then `Extract Module` for each use. This gives you ideas about how to further break up the class.

## Long Parameter List

* Long parameter lists are remnant of procedural programming where routines needed to be passed in all the context they'd need explicitly.
* With objects, we can ask other objects for what we need. 
  * So we should _only pass in enough that the method can get what it needs_
* Use `Replace Parameter with Method` when you can get data for a parameter by making a request to an object you already know about.
* `Preserve Whole Object` to take a bundle of data gathered from an object and replace it with the object itself.
* For several data items with no logical object, consider clumping them with `Introduce Parameter Object`. 

## Divergent Change

* Happens when one class is commonly changed in different ways for different reasons.
  * Related to _single responsibility principle_: an object should have _one reason to change_. 
* Any change to handle a variation should change a single class or module.
* To clean up divergent changes, `Extract Class` for each cause of change.

## Feature Envy

* A method 'seems more interested in a class other than the one it is in'. 
* Use `Move Method` to get method to where it belongs.
* If a method has many dependencies on outside objects, this is a sign.
* What if method relies on data from multiple classes?
  * Which source of data is relied on the most? Move the method there.
* Put things together that change together. 
  * Data and behaviour usually change together. 
    * But not always... Sometimes behaviour differs with same data (e.g. different algorithms for presenting stuff). In this case you can use e.g. strategy/state patterns to vary the behaviour.

## Data Clumps

* If you see the same few data items clumped together in many different contexts, might be worth `Extract Class` to group them. 
* For methods, groups of parameters that appear often together can be grouped using `Introduce Parameter Object` or `Preserve Whole Object`. 
* Test: consider deleting one of the data values. If you did this, would the others make any sense? If not, you should extract to an object.

## Primitive Obsession

* Any time you find yourself relying on comparing primitive values (e.g. switching on a string value) you have primitive obsession.
* Using primitives to determine behaviour is a sign you should extract a 'smarter' object around the primitive.
* Conditionals depending on a type or status code: `Replace Type Code with Polymorphism`, `Replace Type Code with Module Extension`, or `Replace Type Code with State/Strategy`. 
* If you find yourself iterating over an array and supplying complex behaviour, extract it. Use `Replace Array with Object`.

## Case Statements

* Fundamental problem with case statements is duplication (have to change each case statement everywhere it appears in the program). Another problem is these are crappy to read when they get large.
  * Polymorphism handles this nicely
* The case statement matches on a type code. You want a method or class to host the type code. 
  * Use `Extract Method` to extract the case statement, then `Move Method` to get it to ghe class where polymorphism is needed. 
    * Then apply `Replace Type Code with Polymorphism`, `Replace Type Code with Module Extension`, or `Replace Type Code with State/Strategy`.
* If you only have a few cases and the case statement affects only a single method, this is overkill. 
* If one of your conditional cases is null, you can use `Introduce Null Object`. 

## Parallel Inheritance Hierarchies

* Every time you make a subclass of one class, you have to make a subclass of another. 
  * e.g. you have a bunch of class hierarchies doing different stuff but that are prefixed/postfixed with the same thing, indicating a relationship.

## Lazy Class

* A class costs money to maintain and understand. A class that isn't doing enough to pay for itself should be eliminated. 
* `Collapse Hierarchy` if this is a subclass/module.
* Useless components get `Inline Class` or `Inline Module`

## Speculative Generality

* "We might eventually need this general type of thing some day"
* You are effectively developing against requirements that aren't known and don't exist.
* Same as Lazy Class, use `Collapse Hierarchy` if possible. Remove delegation with `Inline Class`. 
* Often found when only users of the class/method are test cases.

## Temporary Field

* An object in which an instance variable is set only in certain circumstances.
  * Difficult to understand -- an object should _need_ all of its variables.
  * Suggests a specialized object waiting to be created.
* Use `Extract Class`. Can eliminate conditional code with `Introduce Null Object`. 
* Common case occurs when class implements a complex algorithm somewhere. To avoid passing a huge parameter list, developer stuffed a bunch of stuff only ever used by the algorithm into ivars. Ivars are only valid during the algorithm, so they aren't really suited to be ivars, and make it really difficult to understand the behaviour of the class.

## Message Chains

* Law of Demeter (principle of least privilege) violation.
* `long.chains.of.method.calls`
* Client is coupled to the _structure_ of the navigation. Any change to intermediate relationships causes the client to have to change.
* Use `Hide Delegate`. `Move Method` in some cases.

## Middle Man

* If e.g. a class is overwhelmingly delegating implementation to some other class, it might be better for callers to just talk to the other class.
* `Remove Middle Man`.
* If only a few methods aren't doing much, you can use `Inline Method`.
* If there is additional behaviour, `Replace Delegation with Hierarchy` turns the real object into a module and includes it in the middleman.
  * Allows extending behaviour without chasing delegation.

## Inappropriate Intimacy

* Look for bidirectional associations. `Change Bidirectional Association to Unidirectional`. 
* Extract common interests from 2 classes to a new class using `Extract Class`. 
* `Hide Delegate` to allow another class to broker the relationship. 
* `Replace Inheritance with Delegation` if subclasses know too much about parent classes.

## Alternative Classes with Different Interfaces

* e.g. methods that do the same thing but have different signatures. 
  * Use `Rename Method`

## Incomplete Library Class

* If functionality on a class should be fundamental, use `Move Method` to move it to the library class.

## Data Class

* Have attributes and nothing else. Dumb data containers. 
* `Remove Setting Method` on any ivar that shouldn't be changed.
* `Encapsulate Collection` on any collection ivars that aren't encapsulated.
* Look at other classes to see what relies on the data. Try to move the behaviour from calling classes to the data class (`Move Method`).
> Data classes are like children. They are okay as a starting point, but to participate as a grownup object, they need to take some responsibility.

## Refused Bequest

* Subclasses that don't need or want some of the functionality provided by parent.
* Create a new sibiling class and use `Push Down Method` to push unused methods to the sibiling. Parent class should only hold what is common. 
* "Nine times out of ten this smell is too faint to be worth cleaning."
* If child classes aren't reusing public interface of parent this is a stronger smell. Remove by using `Replace Inheritance with Delegation`.

## Comments

* If you need to explain _what_ code is doing, it's likely bad code.
* Sometimes you can get rid of a comment just by `Extract Method` to a well-named method.
* Only comment to say _why_ you did something.

## Repetitive Boilerplate

* Analogous to duplication within methods, but applied to class functionality.
* There are some kinds of methods that are so common they can be abstracted. e.g. `attr_reader` in ruby.
* `Introduce Class Annotation` == annotating a class by calling a class method from the class definition. 
  * If the purpose of the code can be captured clearly in a declarative statement this can work well.
  